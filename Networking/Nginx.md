# Nginx

## What is it?
- Nginx (pronounced "engine-x") is a **web server and reverse proxy**
- It sits in front of your app and handles incoming web traffic
- Common uses: serve static files, reverse proxy to backend, SSL termination, load balancing

## What Can Nginx Do?
- **Web Server**: Serve HTML, CSS, JS, images directly
- **Reverse Proxy**: Forward requests to your backend app (Node, Python, etc.)
- **SSL Termination**: Handle HTTPS, pass plain HTTP to your backend
- **Load Balancer**: Distribute traffic across multiple backend servers
- **Rate Limiting**: Limit requests per IP

## Basic Config Structure
```nginx
# /etc/nginx/sites-available/my-app
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;  # Your Node/Python app
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

## Serve Static Files
```nginx
server {
    listen 80;
    server_name yourdomain.com;
    root /var/www/html;     # Where your files are
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;  # SPA fallback
    }
}
```

## Reverse Proxy + HTTPS
```nginx
server {
    listen 443 ssl;
    server_name yourdomain.com;

    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

    location / {
        proxy_pass http://localhost:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $remote_addr;
    }

    location /api {
        proxy_pass http://localhost:8000;  # Different backend for API
    }
}

server {
    listen 80;
    return 301 https://$host$request_uri;  # Force HTTPS
}
```

## WebSocket Proxy
```nginx
location /ws {
    proxy_pass http://localhost:8080;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
}
```

## Useful Commands
```bash
# Test config for errors
nginx -t

# Reload config (without restart)
nginx -s reload

# Enable a site
ln -s /etc/nginx/sites-available/my-app /etc/nginx/sites-enabled/

# Start/stop/restart
systemctl start nginx
systemctl restart nginx
systemctl status nginx

# View logs
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log
```

## Good to Know
- Config files in `/etc/nginx/sites-available/` — enable with symlink to `sites-enabled/`
- Always run `nginx -t` before reloading to catch errors
- Nginx is much faster than Apache for serving static content
- Use **Certbot** with Nginx for automatic HTTPS/Let's Encrypt setup
