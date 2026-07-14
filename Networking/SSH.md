# SSH — Secure Shell

## What is it?
- SSH is a protocol for **securely connecting to remote computers** over a network
- Gives you a command-line terminal on a remote machine
- All communication is **encrypted**
- Default port: **22**
- Think of it as a secure telephone line to a remote computer

## How Authentication Works

### Password Authentication
```bash
ssh username@server-ip
# Enter password when prompted
```

### Key-Based Authentication (More Secure)
- Uses a **key pair**: private key (you keep) + public key (on server)
- No password needed — like having a special lock and key
```bash
# Generate a key pair
ssh-keygen -t ed25519 -C "your-email@example.com"
# Creates: ~/.ssh/id_ed25519 (private) and ~/.ssh/id_ed25519.pub (public)

# Copy public key to server
ssh-copy-id username@server-ip

# Now connect without password
ssh username@server-ip
```

## Common SSH Commands
```bash
# Connect to a server
ssh user@192.168.1.100

# Connect on a custom port
ssh -p 2222 user@server-ip

# Connect using a specific key file
ssh -i ~/.ssh/my-key.pem user@server-ip

# Run a command on remote server without interactive shell
ssh user@server-ip 'ls /var/log'

# Copy file TO remote server
scp localfile.txt user@server-ip:/remote/path/

# Copy file FROM remote server
scp user@server-ip:/remote/file.txt ./local-path/

# Copy entire directory recursively
scp -r ./my-folder user@server-ip:/remote/path/
```

## SSH Config File (~/.ssh/config)
```
# Save connection settings for easy reuse
Host my-server
    HostName 203.0.113.5
    User ubuntu
    IdentityFile ~/.ssh/my-key.pem
    Port 22
```
```bash
# Now connect with just:
ssh my-server
```

## SSH Tunneling (Port Forwarding)
```bash
# Access a remote service as if it's local
# e.g., access DB on remote server via localhost:5433
ssh -L 5433:localhost:5432 user@server-ip
```

## Good to Know
- Disable password auth in production — use keys only
- Change default port 22 to something else to reduce bot attacks
- `~/.ssh/known_hosts` stores fingerprints of servers you've connected to
- Never share your **private key** — only share the `.pub` file
