---
tags: [mcp, auth, oauth, security, protocols]
created: 2026-02-25
difficulty: intermediate
status: complete
---

# MCP Authentication & Authorization

MCP auth is how AI agents prove who they are (and what they're allowed to do) when connecting to remote MCP servers. The spec standardizes on OAuth 2.1 for HTTP-based transports, but in practice the ecosystem uses everything from full OAuth flows to simple API keys to no auth at all.

## Spec Evolution Timeline

| Date | Change |
|---|---|
| March 2025 | Initial MCP auth spec, SSE transport, Dynamic Client Registration (DCR) as default |
| June 2025 | Streamable HTTP replaces SSE, servers classified as OAuth Resource Servers, RFC 8707 Resource Indicators mandatory, RFC 9728 Protected Resource Metadata required |
| November 2025 | Client ID Metadata Documents (CIMD) becomes default over DCR, step-up authorization formalized, PKCE mandatory, OpenID Connect Discovery support added |
| January 2026 | DPoP extension in active development, enterprise hardening ongoing |

## Auth Architecture (Three Roles)

```mermaid
graph LR
    Client[MCP Client] -->|Bearer token| RS[MCP Server<br/>OAuth Resource Server]
    Client -->|Auth code + PKCE| AS[Authorization Server]
    AS -->|Access token| Client
    RS -.->|Validates token| AS
```

- **MCP Client** = OAuth 2.1 client (Claude, Cursor, custom agent)
- **MCP Server** = OAuth 2.1 Resource Server (validates tokens, serves tools)
- **Authorization Server** = Issues tokens, handles consent (can be same host or separate)

## Method 1: OAuth 2.1 (The Spec Standard)

This is the official, spec-mandated approach for HTTP-based MCP transports. Authorization is optional, but when implemented, it MUST follow OAuth 2.1.

### Discovery Flow

The full discovery sequence before any OAuth flow begins:

1. Client sends unauthenticated request to MCP server
2. Server returns `401 Unauthorized` with `WWW-Authenticate` header containing `resource_metadata` URL
3. Client fetches Protected Resource Metadata (RFC 9728) to find the authorization server
4. Client fetches Authorization Server Metadata (RFC 8414 or OpenID Connect Discovery)
5. Client performs OAuth 2.1 authorization flow

### Discovery Endpoints

```
# Protected Resource Metadata (MCP server publishes this)
GET /.well-known/oauth-protected-resource/path/to/mcp
# or at root:
GET /.well-known/oauth-protected-resource

# Authorization Server Metadata (auth server publishes this)
GET /.well-known/oauth-authorization-server
GET /.well-known/oauth-authorization-server/tenant1   # with path
GET /.well-known/openid-configuration                  # OIDC fallback
```

### The 401 Response That Starts Everything

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer resource_metadata="https://mcp.example.com/.well-known/oauth-protected-resource",
                         scope="files:read"
```

### Resource Indicators (RFC 8707) -- Mandatory

Every token request MUST include a `resource` parameter identifying the target MCP server. This prevents token misuse across services:

```
POST /token
  grant_type=authorization_code
  &code=abc123
  &code_verifier=dBjftJeZ4CVP...
  &resource=https%3A%2F%2Fmcp.example.com
```

### PKCE -- Mandatory

All MCP clients MUST use PKCE with `S256` challenge method. Clients MUST verify the auth server supports PKCE via `code_challenge_methods_supported` in metadata before proceeding.

### Token Usage

```http
GET /mcp HTTP/1.1
Host: mcp.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

Every HTTP request must include the Authorization header, even within the same logical session.

### Step-Up Authorization

New in November 2025. When a client has a valid token but needs more permissions:

1. Server returns `403 Forbidden` with `insufficient_scope` error
2. Client reads required scopes from the `WWW-Authenticate` header
3. Client initiates a new auth flow requesting the elevated scopes
4. Client retries the original request with the new token

```http
HTTP/1.1 403 Forbidden
WWW-Authenticate: Bearer error="insufficient_scope",
                         scope="files:read files:write user:profile",
                         resource_metadata="https://mcp.example.com/.well-known/oauth-protected-resource"
```

## Method 2: Client Registration Approaches

Three ways for a client to identify itself to an auth server, in priority order:

### 1. Pre-registration (Highest Priority)

Client has a hardcoded `client_id` for a known auth server. Simplest, used when client and server have a prior relationship.

### 2. Client ID Metadata Documents (Default Since Nov 2025)

The client uses an HTTPS URL as its `client_id`. The auth server fetches a JSON metadata document from that URL to verify the client:

```json
{
  "client_id": "https://app.example.com/oauth/client-metadata.json",
  "client_name": "My MCP Client",
  "redirect_uris": ["http://127.0.0.1:3000/callback"],
  "grant_types": ["authorization_code"],
  "response_types": ["code"],
  "token_endpoint_auth_method": "none"
}
```

This "pull" model (server fetches client info) replaced the "push" model (client registers itself) as the default. No server-side registration logic needed.

### 3. Dynamic Client Registration (Deprecated Default)

RFC 7591. Client POSTs to a `/register` endpoint to get credentials. Changed from SHOULD to MAY in November 2025 -- now a backwards-compatibility fallback only.

## Method 3: API Keys and Bearer Tokens (Practical Reality)

The spec says OAuth 2.1, but many production MCP servers use simpler approaches:

| Pattern | How it works | Example |
|---|---|---|
| API key in header | `Authorization: Bearer sk-xxx` or custom header | Atlassian Rovo MCP, many custom servers |
| API key from environment | Server reads key from env var at startup | Local stdio MCP servers |
| Token passthrough | Client passes a pre-existing API token to the MCP server | Common but **anti-pattern** per spec |

For **stdio transports**, the spec explicitly says: retrieve credentials from the environment, do NOT use the OAuth flow.

### Production Server Reality (518 servers surveyed)

- 59% have some form of auth (OAuth, API keys, or bearer tokens)
- 41% have no authentication at all
- ~30% respond to any tool call without tokens or verification
- Even "authenticated" servers often allow unauthenticated `tools/list` calls

## Method 4: mTLS (Mutual TLS)

Not part of the MCP spec, but used in enterprise/zero-trust deployments. Both client and server present TLS certificates, providing mutual authentication at the transport layer.

- Requires PKI infrastructure (certificate authority, cert distribution)
- Stronger than bearer tokens (cryptographic client identity)
- Often combined with OAuth (mTLS for transport auth, OAuth for authorization)
- Relevant RFC: OAuth 2.0 Mutual-TLS Client Authentication (RFC 8705)

## Method 5: DPoP (Demonstrating Proof-of-Possession)

Active MCP spec enhancement proposal as of January 2026. DPoP binds access tokens to the specific client that requested them, preventing token theft/replay:

- Works at the application layer (no PKI needed, unlike mTLS)
- Client generates a key pair and includes a signed proof in each request
- Auth server binds the token to the client's public key
- If someone steals the token, they can't use it without the private key

Not yet in the released MCP spec but being actively developed.

## Cloudflare Workers MCP Auth

Cloudflare's `@cloudflare/workers-oauth-provider` is a TypeScript library that wraps your Worker, implementing the provider side of OAuth 2.1. Your MCP server receives authenticated user details as a parameter -- you don't manage tokens directly.

### Four Approaches

```mermaid
graph TD
    W[Your MCP Worker] --> OA[OAuth Provider Library]
    OA --> A[Cloudflare Access<br/>SSO + Access policies]
    OA --> B[Third-Party OAuth<br/>GitHub, Google, etc.]
    OA --> C[Auth-as-a-Service<br/>Auth0, Stytch, WorkOS]
    OA --> D[Self-Contained<br/>Worker handles auth itself]
```

### What the Library Implements

- OAuth 2.1 with PKCE
- RFC 8414 (Authorization Server Metadata)
- RFC 9728 (Protected Resource Metadata)
- RFC 7591 (Dynamic Client Registration)
- Client ID Metadata Documents (HTTPS URLs as client IDs)

### Storage

All token data stored in Workers KV (`OAUTH_KV` binding). Security design:
- Tokens and codes stored as **hashes only** (no plaintext secrets)
- Grant props encrypted using token material as key
- User IDs and metadata unencrypted (for audit/revocation)
- Refresh token strategy: 2 simultaneous valid tokens per grant (prevents DoS from failed refreshes)

### Auth Context in Handlers

```typescript
// With McpAgent class
class MyMCP extends McpAgent<Env, {}, AuthProps> {
  async init() {
    // this.props contains authenticated user info
    const userId = this.props.userId;
  }
}

// With createMcpHandler
const handler = createMcpHandler({
  tools: {
    myTool: async () => {
      const auth = getMcpAuthContext(); // from AsyncLocalStorage
    }
  }
});
```

### Permission-Based Tool Access

Two patterns:
1. **Check inside handler** -- tool is visible but returns denial message if unauthorized
2. **Conditional registration** -- tool never appears to the LLM if user lacks permission

## Security Considerations

### Token Audience Validation (Critical)

MCP servers MUST validate that tokens were issued specifically for them. Without this:
- A token stolen from one service could access another
- "Confused deputy" attacks where a proxy MCP server forwards tokens to downstream APIs

### Token Passthrough (Anti-Pattern)

The spec explicitly forbids MCP servers from accepting client tokens and passing them through to downstream APIs. If your MCP server calls upstream APIs, it must use **separate tokens** obtained via its own OAuth client credentials.

### Scope Minimization

`scopes_supported` in Protected Resource Metadata should represent the **minimal** set for basic functionality. Additional scopes come via step-up authorization.

## When to Use What

| Scenario | Recommended Auth |
|---|---|
| Remote MCP server, general purpose | OAuth 2.1 (full spec compliance) |
| Local stdio server | Environment variables (API keys, tokens) |
| Enterprise/internal deployment | OAuth 2.1 + mTLS or Cloudflare Access |
| Quick prototype / personal tool | API key in header (Bearer token) |
| Cloudflare Worker MCP server | `workers-oauth-provider` library |
| High-security (financial, healthcare) | OAuth 2.1 + DPoP (when available) or mTLS |

## Related

- [[mcp]] -- MCP overview, Code Mode, Server Portals
- [[cloudflare/workers]] -- Cloudflare Workers platform

## References

- [MCP Authorization Spec (draft)](https://modelcontextprotocol.io/specification/draft/basic/authorization)
- [MCP Authorization Spec (2025-11-25)](https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization)
- [MCP Auth Extensions Repository](https://github.com/modelcontextprotocol/ext-auth)
- [Cloudflare workers-oauth-provider](https://github.com/cloudflare/workers-oauth-provider)
- [Cloudflare MCP Authorization Docs](https://developers.cloudflare.com/agents/model-context-protocol/authorization/)
- [November 2025 MCP Auth Spec Analysis](https://den.dev/blog/mcp-november-authorization-spec/)
- [Auth0: MCP Spec Updates from June 2025](https://auth0.com/blog/mcp-specs-update-all-about-auth/)
- [Client Registration in November 2025 Spec](https://aaronparecki.com/2025/11/25/1/mcp-authorization-spec-update)
- [MCP January 2026 Core Maintainer Update](https://blog.modelcontextprotocol.io/posts/2026-01-22-core-maintainer-update/)
- [Stack Overflow: MCP Authentication and Authorization](https://stackoverflow.blog/2026/01/21/is-that-allowed-authentication-and-authorization-in-model-context-protocol)
- [Production MCP Server Auth Survey (518 servers)](https://dev.to/kai_security_ai/authentication-in-mcp-what-518-production-servers-actually-do-2a63)
- [Stytch: MCP Auth Implementation Guide](https://stytch.com/blog/MCP-authentication-and-authorization-guide/)
- [Descope: Diving Into the MCP Authorization Spec](https://www.descope.com/blog/post/mcp-auth-spec)
