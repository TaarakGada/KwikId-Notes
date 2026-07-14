# SFTP vs HTTP vs HTTPS — Comparison

## Purpose Comparison
| Protocol | Primary Use |
|----------|-------------|
| **HTTP** | Browse websites, REST APIs, web data transfer |
| **HTTPS** | Same as HTTP but encrypted |
| **SFTP** | Secure file transfer to/from servers |

## Side-by-Side Comparison
| Feature | HTTP | HTTPS | SFTP |
|---------|------|-------|------|
| Port | 80 | 443 | 22 |
| Encryption | ❌ No | ✅ Yes (TLS) | ✅ Yes (SSH) |
| Primary Use | Web requests | Secure web requests | File transfer |
| Auth Method | None / tokens | None / tokens / certs | Password / SSH keys |
| Human-facing | Yes (browser) | Yes (browser) | No (terminal/client) |
| Stateless | Yes | Yes | No (session-based) |
| File Transfer | Possible | Possible | Native purpose |

## When to Use Each

### Use HTTP when:
- Building internal APIs on a trusted network
- Development/local testing

### Use HTTPS when:
- Any public-facing website or API
- Handling user data, logins, payments
- Accessing browser APIs (geolocation, camera)

### Use SFTP when:
- Uploading files to a server (deployment, backups)
- Giving partners secure access to deliver files
- Managing server files remotely
- Transferring large files securely

## Real World Examples
- **HTTP/HTTPS**: Your React app calling `/api/users` — use HTTPS
- **SFTP**: DevOps engineer uploading build artifacts to EC2 — use SFTP
- **HTTPS**: User submitting a login form — always HTTPS
- **SFTP**: Automated backup script uploading DB dumps to a storage server

## Quick Rule of Thumb
> Use **HTTPS** for anything in a browser
> Use **SFTP** for uploading/downloading files to a server
> Use **HTTP** only in local development
