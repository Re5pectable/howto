# Nginx with Certbot and reverse proxy to a local port

Before you start:
- Replace `example.com` with your real domain.
- Make sure the domain already points to this server via DNS (`A`/`AAAA` record).

```bash
sudo apt update
sudo apt install -y nginx certbot python3-certbot-nginx

sudo systemctl enable --now nginx
sudo ufw allow 'Nginx Full'

sudo nano /etc/nginx/sites-available/example.com
```

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://127.0.0.1:8000;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

```bash
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

sudo certbot --nginx -d example.com

systemctl list-timers | grep certbot
sudo certbot renew --dry-run
```
