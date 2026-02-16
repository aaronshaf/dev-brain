---
tags: [diagrams, visualization, markdown, documentation]
created: 2026-02-15
difficulty: beginner
status: complete
---

# Mermaid Diagrams

Text-based diagramming and charting tool that renders in Markdown. Supported in GitHub, GitLab, Obsidian, and many other platforms.

## Supported Diagram Types

| Type | Use Case | Syntax |
|------|----------|--------|
| **Flowchart** | Process flows, algorithms | `graph TD` / `graph LR` |
| **Sequence** | Interactions between actors | `sequenceDiagram` |
| **Class** | UML class diagrams | `classDiagram` |
| **State** | State machines | `stateDiagram-v2` |
| **Entity Relationship** | Database schemas | `erDiagram` |
| **Gantt** | Project timelines | `gantt` |
| **Pie** | Proportions | `pie` |
| **Git Graph** | Git branching | `gitGraph` |
| **User Journey** | User experience flows | `journey` |
| **Quadrant** | 2x2 matrices | `quadrantChart` |
| **Timeline** | Chronological events | `timeline` |

## Common Diagrams

### Flowchart
```mermaid
graph TD
    A[Start] --> B{Decision}
    B -->|Yes| C[Action 1]
    B -->|No| D[Action 2]
    C --> E[End]
    D --> E
```

**Syntax:**
- `graph TD` = top-down, `graph LR` = left-right
- `[ ]` = rectangle, `{ }` = diamond, `(( ))` = circle
- `-->` = arrow, `-.->` = dotted, `==>` = thick

### Sequence Diagram
```mermaid
sequenceDiagram
    participant User
    participant API
    participant DB

    User->>API: POST /login
    API->>DB: Query user
    DB-->>API: Return data
    API-->>User: JWT token
```

**Syntax:**
- `->>` = solid arrow, `-->>` = dashed response
- `activate`/`deactivate` for lifelines
- `Note over` for annotations

### Class Diagram
```mermaid
classDiagram
    class User {
        +String email
        +String name
        +login()
        +logout()
    }
    class Post {
        +String title
        +DateTime created
        +publish()
    }
    User "1" --> "*" Post : creates
```

**Syntax:**
- `+` = public, `-` = private, `#` = protected
- `-->` = association, `--|>` = inheritance
- `"1" --> "*"` = cardinality

### State Diagram
```mermaid
stateDiagram-v2
    [*] --> Idle
    Idle --> Processing: start
    Processing --> Success: complete
    Processing --> Failed: error
    Success --> [*]
    Failed --> Idle: retry
```

### Entity Relationship
```mermaid
erDiagram
    USER ||--o{ POST : creates
    USER {
        int id PK
        string email UK
        string name
    }
    POST {
        int id PK
        int user_id FK
        string title
        datetime created
    }
```

**Relationships:**
- `||--||` = one to one
- `||--o{` = one to many
- `}o--o{` = many to many
- `PK` = primary key, `FK` = foreign key

### Gantt Chart
```mermaid
gantt
    title Project Timeline
    dateFormat YYYY-MM-DD
    section Phase 1
    Planning           :a1, 2026-01-01, 30d
    Design            :a2, after a1, 20d
    section Phase 2
    Development       :a3, after a2, 60d
    Testing           :a4, after a3, 30d
```

### Git Graph
```mermaid
gitGraph
    commit
    branch develop
    checkout develop
    commit
    commit
    checkout main
    merge develop
    commit
```

## Tips

### Styling
```mermaid
graph LR
    A[Normal]
    B[Styled]:::highlight

    classDef highlight fill:#f9f,stroke:#333,stroke-width:4px
```

### Subgraphs
```mermaid
graph TB
    subgraph Frontend
        A[React]
        B[Redux]
    end
    subgraph Backend
        C[API]
        D[DB]
    end
    A --> C
    B --> C
    C --> D
```

### Links and Tooltips
```mermaid
graph LR
    A[GitHub]
    click A "https://github.com" "Go to GitHub"
```

## Platform Support

### GitHub
- Native support in `.md` files and comments
- Wrap in triple backticks with `mermaid` language tag
- Max 50KB per diagram

### Obsidian
- Native support via triple backticks
- Live preview rendering
- Export to PNG/SVG

### Common Issues
- **Too complex**: Break into multiple diagrams
- **Syntax errors**: Use [Mermaid Live Editor](https://mermaid.live) to validate
- **Not rendering**: Check platform support and version

## Use Cases

**Architecture docs:**
```mermaid
graph TB
    Client[Web Client]
    LB[Load Balancer]
    API1[API Server 1]
    API2[API Server 2]
    Cache[(Redis)]
    DB[(PostgreSQL)]

    Client --> LB
    LB --> API1
    LB --> API2
    API1 --> Cache
    API2 --> Cache
    API1 --> DB
    API2 --> DB
```

**API flows:**
```mermaid
sequenceDiagram
    Client->>+Auth: POST /login
    Auth->>+DB: Verify credentials
    DB-->>-Auth: User data
    Auth-->>-Client: Access token
    Client->>+API: GET /data (with token)
    API->>API: Validate token
    API->>+DB: Query data
    DB-->>-API: Results
    API-->>-Client: JSON response
```

**State machines:**
```mermaid
stateDiagram-v2
    [*] --> Draft
    Draft --> Review: submit
    Review --> Approved: approve
    Review --> Rejected: reject
    Rejected --> Draft: revise
    Approved --> Published: publish
    Published --> Archived: archive
    Archived --> [*]
```

## Quick Reference

| Need | Diagram Type |
|------|--------------|
| Algorithm logic | Flowchart |
| API interactions | Sequence |
| Code structure | Class |
| Workflow states | State |
| Data model | ER Diagram |
| System architecture | Flowchart with subgraphs |
| Project schedule | Gantt |

## Related Topics

- [[documentation]] - General documentation practices
- [[architecture]] - System design patterns
- [[project-structure]] - Code organization

## References

- [Mermaid Official Docs](https://mermaid.js.org/)
- [GitHub Mermaid Support](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-diagrams)
- [Mermaid Live Editor](https://mermaid.live) - Test and validate diagrams
