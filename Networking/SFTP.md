# SFTP — SSH File Transfer Protocol

## What is it?
- SFTP is a **secure file transfer protocol** that runs over SSH
- Used to upload/download files to/from a remote server
- All data is encrypted (unlike FTP which is plaintext)
- Operates over port **22** (same as SSH)
- Think of it as a secure file manager for remote servers

## SFTP vs FTP
| | SFTP | FTP |
|--|------|-----|
| Encryption | Yes (via SSH) | No (plaintext) |
| Port | 22 | 21 |
| Authentication | SSH keys or password | Password |
| Firewall-friendly | Yes (one port) | No (multiple ports) |

## Basic SFTP Commands
```bash
# Connect to SFTP server
sftp user@server-ip

# If using a specific key
sftp -i ~/.ssh/my-key.pem user@server-ip
```

## Once Connected (SFTP Shell)
```bash
# Show remote directory
pwd

# List files on remote
ls

# Change remote directory
cd /var/www/html

# Show local directory (prefixed with l)
lpwd
lls
lcd ~/Downloads

# Upload a file (local → remote)
put localfile.txt
put localfile.txt /remote/path/newname.txt

# Download a file (remote → local)
get remotefile.txt
get /remote/path/file.txt ~/Downloads/

# Upload entire directory
put -r ./my-folder

# Download entire directory
get -r /remote/folder

# Exit
bye
```

## Using scp (simpler for quick transfers)
```bash
# Upload
scp file.txt user@server:/remote/path/

# Download
scp user@server:/remote/file.txt ./

# Upload a folder
scp -r ./folder user@server:/remote/path/
```

## GUI Clients for SFTP
- **FileZilla** — Free, cross-platform
- **WinSCP** — Windows-only, very popular
- **Cyberduck** — Mac/Windows
- **VS Code** — Remote SSH extension lets you browse files

## Good to Know
- SFTP is not related to FTP — it's a completely different protocol
- You need SSH access to use SFTP on a server
- Works great for deployment: uploading built files to a server
- Some cloud storage services expose SFTP endpoints
