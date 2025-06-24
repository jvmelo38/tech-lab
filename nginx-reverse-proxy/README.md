# 🔁 NGINX - Reverse Proxy Guide

This guide explains how to configure NGINX as a reverse proxy to redirect traffic from a public domain to a local service.

---

## 🌐 What is a Reverse Proxy?

A reverse proxy accepts client requests and forwards them to an internal server. It helps with:

- Exposing a web app running on another port (e.g., :3000)
- Applying HTTPS even if the app runs in plain HTTP
- Hiding internal structure
- Load balancing

---

## ⚙️ Sample Configuration

Here’s a basic NGINX configuration example:

```nginx
server {
    listen 443 ssl;
    server_name app.example.com;

    ssl_certificate     /etc/nginx/ssl/cert.pem;
    ssl_certificate_key /etc/nginx/ssl/key.pem;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
