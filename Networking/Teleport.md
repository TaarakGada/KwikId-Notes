# Teleport

## What is it?
- Teleport is an **open-source access management platform** for infrastructure
- It provides secure SSH, Kubernetes, database, and web app access — all in one
- Think of it as: SSH + VPN + audit logging, all unified
- Solves the problem of managing SSH keys across many servers

## Why Use Teleport?
- Instead of managing SSH keys on every server, all access goes through **Teleport**
- Built-in **audit logs** — who accessed what, when, and what commands they ran
- **MFA support** — require 2FA for all access
- Session recording — record and replay terminal sessions
- No need to distribute SSH private keys to team members

## Key Components
- **Teleport Auth Server**: The identity and policy center
- **Teleport Proxy**: The public-facing gateway
- **Teleport Node**: Agent running on each server you want to manage
- **tsh**: The CLI tool users use to connect

## How It Works
```
Developer
    ↓  tsh login
Teleport Proxy (public)
    ↓  verify identity
Teleport Auth Server
    ↓  issues short-lived certificate
Developer gets temporary SSH cert
    ↓  tsh ssh user@server
Secure connection to server via Teleport Node
```

## Basic Usage (tsh CLI)
```bash
# Login to Teleport cluster
tsh login --proxy=teleport.yourdomain.com

# List accessible servers
tsh ls

# SSH into a server
tsh ssh ubuntu@web-server-1

# List recorded sessions
tsh recordings ls

# Join an ongoing session (pair programming / incident review)
tsh join <session-id>
```

## Good to Know
- Teleport issues **short-lived certificates** (e.g., valid for 12 hours) instead of permanent keys
- Expired certs mean access is automatically revoked
- Teleport Cloud is the hosted version (no self-hosting needed)
- Supports: SSH, Kubernetes, PostgreSQL, MySQL, web apps
- Great for teams that need compliance and audit trails (SOC 2, HIPAA)
- **Role-Based Access Control (RBAC)**: Control who can access which servers
