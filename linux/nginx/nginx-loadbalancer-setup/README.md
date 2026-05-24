# Nginx Load Balancer Setup

## Architecture

- Load Balancer: stlb01
- App Servers:
  - stapp01
  - stapp02
  - stapp03

## Technologies Used

- Nginx
- Apache HTTPD
- Linux

## Nginx Configuration

```nginx
upstream app_servers {
    server 10.244.164.99:3003;
    server 10.244.244.153:3003;
    server 10.244.195.231:3003;
}

server {
    listen 80;

    location / {
        proxy_pass http://app_servers;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## Verification Commands

```bash
sudo nginx -t
sudo systemctl restart nginx
curl http://stlb01:80
```

## Load Balancer Working

Nginx distributes incoming traffic across all backend Apache servers using Round Robin load balancing.
