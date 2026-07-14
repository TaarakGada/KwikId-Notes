# HTTPS — HTTP Secure

## What is it?
- HTTPS = HTTP + **TLS encryption**
- All data is encrypted between client and server — no one can read it in transit
- Uses port **443** by default
- Shown with a padlock icon in browsers

## Why HTTPS?
- **Privacy**: Encrypts data so ISPs, hackers can't read it
- **Integrity**: Data can't be tampered with in transit
- **Authentication**: Proves you're talking to the real server (not an imposter)
- Required for: login pages, payment forms, APIs, geolocation access

## How TLS Works (Simplified)
```
1. Client connects to server
2. Server sends its SSL Certificate (contains public key)
3. Client verifies certificate is valid (signed by trusted CA)
4. Client and server negotiate an encryption key
5. All further communication is encrypted
```

## SSL Certificate
- A digital document proving a server's identity
- Issued by a **Certificate Authority (CA)** like Let's Encrypt, DigiCert
- Contains: domain name, public key, expiry date, CA signature
- **Let's Encrypt** provides **free** SSL certificates

## Setting Up HTTPS with Let's Encrypt (Certbot)
```bash
# Install certbot
sudo apt install certbot python3-certbot-nginx

# Get certificate for your domain
sudo certbot --nginx -d yourdomain.com

# Auto-renew (runs every 12 hours)
sudo certbot renew --dry-run
```

## HTTPS in Nginx Config
```nginx
server {
    listen 443 ssl;
    server_name yourdomain.com;

    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

    location / {
        proxy_pass http://localhost:3000;
    }
}

# Redirect HTTP to HTTPS
server {
    listen 80;
    server_name yourdomain.com;
    return 301 https://$host$request_uri;
}
```

## Good to Know
- Google ranks HTTPS sites higher in search results
- Browsers show "Not Secure" warning for HTTP sites
- Geolocation, camera, microphone APIs require HTTPS
- **HSTS** (HTTP Strict Transport Security) tells browsers to always use HTTPS
