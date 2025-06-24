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

## 🪟 Installation on Windows

Although NGINX was designed for Unix-like systems, a stable version for Windows is available.

Steps:

Go to the official website: https://nginx.org/en/download.html

Download the Mainline version (Windows ZIP archive).

Extract the .zip to a folder like C:\nginx.

Open a Command Prompt or PowerShell and navigate to the folder:

cd C:\nginx
start nginx

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
```

> This will forward HTTPS requests from `https://app.example.com` to a service running on `localhost:3000`.

---

## 🧪 Testing the Configuration

Linux 

```bash
sudo nginx -t
sudo systemctl reload nginx
```
Windows 

```bash 

start ngix
./nginx -s reload

```

Use `curl -I https://app.example.com` to verify the response headers.

Or access the page on the unnamed DNS via browser

---

## 🔒 Free SSL with Let's Encrypt (Opicional)

To generate a free SSL certificate and apply it automatically:

```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx
```

---

## 🗂️ Suggested File Structure

```
nginx-reverse-proxy/
├── README.md
├── config/
│   └── example.conf
└── ssl/
    ├── cert.pem
    └── key.pem
```

---

## 👤 Author

**João Melo**  
Cloud Solutions Architect  
https://github.com/jvmelo38/
