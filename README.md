# DevJungle: Linux-Based Server Setup (Ubuntu)

![Devjungle logo](DevJungle.webp)

Welcome to **DevJungle**, where we **Code \~ Evolve \~ Thrive**! 🚀

This guide will walk you through setting up an Nginx web server on a Linux-based OS (Ubuntu). Follow along step-by-step to get your server running smoothly!

> **Note:** Feel free to fork this project and improve it—your contributions are welcome! 😊

---

## 📌 Table of Contents

1. [Installing Nginx](#installing-nginx)
   - [Method 1: Using Homebrew (Recommended)](#method-1-using-homebrew-recommended)
   - [Method 2: Manual Installation](#method-2-manual-installation)
2. [Understanding Nginx Directory Structure](#understanding-nginx-directory-structure)
3. [Starting, Stopping, and Reloading Nginx](#starting-stopping-and-reloading-nginx)
4. [Viewing the Default Nginx Page](#viewing-the-default-nginx-page)
5. [Serving Static Content](#serving-static-content)
   - [Setting Up a Website Directory](#setting-up-a-website-directory)
   - [Configuring Nginx to Serve Your Site](#configuring-nginx-to-serve-your-site)
6. [Common Errors & Fixes](#common-errors--fixes)
   - [Port Already in Use](#port-already-in-use)
7. [Handling MIME Types](#handling-mime-types)
   - [Manual Configuration](#manual-configuration)
   - [Using Predefined MIME Types](#using-predefined-mime-types)

---

## 1️⃣ Installing Nginx

### Method 1: Using Homebrew (Recommended)

#### Step 1: Install Homebrew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

#### Step 2: Install Nginx

```bash
brew install nginx
```

### Method 2: Manual Installation

#### Step 1: Download Nginx

[Click here to download Nginx](http://nginx.org/en/download.html)

#### Step 2: Follow Official Installation Instructions

Refer to the [Nginx documentation](https://nginx.org/en/docs/) for step-by-step guidance.

---

## 2️⃣ Understanding Nginx Directory Structure

Nginx configuration files are typically located in one of these directories:

- `/usr/local/nginx/conf`
- `/etc/nginx`
- `/usr/local/etc/nginx`

The main configuration file is **nginx.conf**.

---

## 3️⃣ Starting, Stopping, and Reloading Nginx

### Start Nginx

```bash
sudo nginx
```

### Stop or Reload Nginx

```bash
sudo nginx -s stop   # Fast shutdown
sudo nginx -s quit   # Graceful shutdown
sudo nginx -s reload # Reload configuration
```

---

## 4️⃣ Viewing the Default Nginx Page

Open your browser and visit:

```
http://YOUR_SERVER_IP/
```

If using a VPS without a domain, replace `YOUR_SERVER_IP` with your actual server IP.

---

## 5️⃣ Serving Static Content

### Setting Up a Website Directory

Navigate to the web root directory:

```bash
cd /var/www
```

Create a new folder for your website:

```bash
sudo mkdir myWebsite
cd myWebsite
```

Create an `index.html` file:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Welcome to My Website</title>
</head>
<body>
    <h1>Hello, World!</h1>
</body>
</html>
```

### Configuring Nginx to Serve Your Site

Edit the **nginx.conf** file:

```bash
sudo nano /etc/nginx/nginx.conf
```

Replace or add the following inside the `server` block:

```nginx
server {
    listen 80;
    root /var/www/myWebsite;
    index index.html;
    location / {
        try_files $uri $uri/ =404;
    }
}
```

Save the file and reload Nginx:

```bash
sudo nginx -s reload
```

---

## 6️⃣ Common Errors & Fixes

### Port Already in Use

If you get an error like `bind() to 0.0.0.0:8080 failed (98: Address already in use)`, follow these steps:

#### Step 1: Find the Process Using Port 8080

```bash
sudo lsof -i :8080
```

#### Step 2: Stop the Process

```bash
sudo kill <PID>
```

Replace `<PID>` with the actual process ID.

#### Step 3: Change the Port (Optional)

Edit `nginx.conf` and update:

```nginx
listen 8081;
```

Then restart Nginx:

```bash
sudo systemctl restart nginx
```

---

## 7️⃣ Handling MIME Types

### Manual Configuration

If CSS styles are not loading, update **nginx.conf**:

```nginx
http {
    types {
        text/css css;
        text/html html;
    }
}
```

### Using Predefined MIME Types

Instead of manually adding MIME types, use the built-in file:

```nginx
http {
    include mime.types;
}
```

This ensures all necessary file types are correctly served.

---

🎉 **You're all set!** Your Nginx server is now up and running. If you found this guide helpful, give it a star ⭐ on GitHub! Happy coding! 😊

