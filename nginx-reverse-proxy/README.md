# 🔁 NGINX - Reverse Proxy Guide

This guide explains how to configure NGINX as a reverse proxy to redirect traffic from a public domain to a local service.

---

## 🌐 What is a Reverse Proxy?

A reverse proxy accepts client requests and forwards them to an internal server. It helps with:

- Exposing a web app running on another port (e.g., :3000)
- Applying HTTPS even if the app runs in plain HTTPs
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

```bash
cd C:\nginx
start nginx
```
⚠️ On Windows, NGINX doesn't run as a service by default. For production, Linux is recommended, However, it can be programmed to start via a schedule task.

## ⚙️ Sample Configuration

Here’s a basic NGINX configuration example:

```nginx
server {
    listen 443 ssl;
    server_name app.example.com;

    ssl_certificate     /etc/nginx/ssl/cert.pem;
    ssl_certificate_key /etc/nginx/ssl/key.pem;

    location / {
        proxy_pass https://localhost:3000;
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

## ⚠️ Attention: server_names_hash_bucket_size

If you're using long domain names or multiple subdomains in your configuration, you might encounter an error like:

```bash
[emerg] could not build server_names_hash
```

To fix this, you can increase the hash bucket size in your nginx.conf:

```bash
http {
    server_names_hash_bucket_size 128;

    ...
}
```

## 🌐 Multiple server {} blocks for multiple domains

NGINX allows you to define multiple server {} blocks, each responding to different domain names (FQDNs).

Example:

```bash
server {
    listen 443;
    server_name site1.example.com;

    location / {
        proxy_pass https://localhost:3000;
    }
}

server {
    listen 443;
    server_name site2.example.com;

    location / {
        proxy_pass https://localhost:4000;
    }
}

```
This way, based on the incoming DNS (e.g., site1.example.com vs site2.example.com), NGINX can route traffic to different applications or services.

---

## 👤 Author

**João Melo**  
Cloud Analyst
https://github.com/jvmelo38/
