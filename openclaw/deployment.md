---
tags: [openclaw, deployment, vps, hetzner, tailscale, security]
created: 2026-02-16
difficulty: intermediate
status: complete
---

# VPS Deployment

Deploying OpenClaw means setting up a cheap Linux server that runs 24/7 to host your AI agent. This guide covers secure VPS setup with Hetzner, Tailscale networking, and hardening practices for production deployments.

## Recommended Host: Hetzner

- **Cost:** $2.50-5/month VPS
- **Specs needed:**
  - 2GB RAM (4GB recommended)
  - 1-2 vCPU
  - 20GB SSD

## Security-First Setup

```bash
# 1. Create VPS with SSH key (disables password login automatically)

# 2. Install Tailscale on both VPS and laptop
curl -fsSL https://tailscale.com/install.sh | sh
tailscale up

# 3. Configure Hetzner firewall
# SSH port: ONLY allow Tailscale subnet (100.64.0.0/10)
# HTTPS port: ONLY allow Cloudflare subnets (if hosting web UI)
# Block all other inbound traffic

# 4. Disable password login (verify)
sudo grep "PasswordAuthentication no" /etc/ssh/sshd_config

# 5. Enable unattended security updates
sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades

# 6. Optional: Change SSH port for security by obscurity
sudo nano /etc/ssh/sshd_config
# Change Port 22 to Port 49201 (or your preferred number)
sudo systemctl restart sshd
```

**Defense in depth layers:**
1. SSH keys only (no passwords)
2. Tailscale firewall (only you can access)
3. Cloudflare firewall for web services
4. Auto-updates with auto-reboot
5. Port obscurity (optional but helpful)

## Alternative: Mac Mini M4

For local deployments, Mac Mini M4 is popular:
- Silent, low power (~20W)
- Native ARM performance
- Easy macOS setup
- Can't be attacked from internet if properly configured

## Related Topics

- [[security]] - Security hardening and threat model
- [[configuration]] - Post-deployment configuration
- [[reliability]] - Monitoring and self-healing
