# Chat Application

## Introduction

This repository contains two versions of a browser-based chat application:

- **index.html**: full-featured, production-ready web chat with security, PWA, internationalization, theming, and offline support  
- **local.html**: simplified local version for development or LAN use, no encryption, HTTP-only, minimal dependencies  

---

## Features

### index.html (Full Version)

- Secure password hashing with bcrypt.js  
- Progressive Web App (service worker) for offline use  
- Lazy-loading and pagination in the global chat  
- Internationalization (English/Italian)  
- Day/night theme switch via CSS variables  
- Single-file deployment: no backend required beyond a static server supporting HTTP PUT/HEAD and HTTPS  

### local.html (Local Version)

- Lightweight, single-file chat interface  
- No password hashing or encryption  
- Stores messages in `messages.json` via HTTP PUT  
- Designed for Apache2 HTTP with WebDAV modules, no HTTPS needed  
- Ideal for quick local development or LAN demos  

---

## Requirements

### Full Version (`index.html`)

- Static file server with HTTPS (Apache2, Nginx, Caddy, etc.)  
- HTTP methods GET, PUT, HEAD enabled on JSON files  

### Local Version (`local.html`)

- Apache2 with WebDAV modules (`dav_fs`, `dav`) installed  
- HTTP PUT support on the chat directory  
- Permissions granting write access to `messages.json`  

---

## Setup and Configuration

### 1. Deploy Files

Place `index.html`, `local.html`, and an initial `messages.json` into your webroot (e.g., `/var/www/html/chat/`).

### 2. Initialize `messages.json`

Create `/var/www/html/chat/messages.json` with:

```json
[]
```

### 3. Configure Apache2 for Local Version

Enable WebDAV modules and restart Apache:

```bash
sudo a2enmod dav_fs
sudo a2enmod dav
sudo systemctl restart apache2
```

Add to your VirtualHost (e.g., `/etc/apache2/sites-available/000-default.conf`):

```apache
<Directory /var/www/html/chat>
  Dav On
  Options Indexes
  Require all granted
</Directory>
```

Reload configuration:

```bash
sudo systemctl reload apache2
```

### 4. Enable HTTPS for Full Version

Obtain and install an SSL certificate (Let’s Encrypt or CA) and ensure `index.html` is served over HTTPS.

---

## Usage

- Visit `https://your-domain/chat/index.html` for the full-featured chat  
- Visit `http://your-local-ip/chat/local.html` for the local version  
- On first load, enter your nickname when prompted  
- Type and send messages; they are persisted in `messages.json`  

---

## Feature Comparison

| Feature                       | index.html (Full)    | local.html (Local)   |
|-------------------------------|----------------------|----------------------|
| HTTPS required                | Yes                  | No                   |
| Password hashing (bcrypt.js)  | Yes                  | No                   |
| PWA offline support           | Yes                  | No                   |
| Lazy-loading & pagination     | Yes                  | No                   |
| Internationalization (EN/IT)  | Yes                  | No                   |
| Day/night theme               | Yes                  | No                   |
| Server dependencies           | Any static HTTPS     | Apache2 + WebDAV     |

---

## Customization

- Add or remove languages in the i18n section of `index.html`  
- Tweak CSS variables in the `<style>` block for custom themes  
- Adjust the polling interval or replace with WebSocket for real-time updates  
- Swap the JSON PUT mechanism for a backend API (PHP, Node.js, etc.)  
- Use localStorage caching for offline message history  

---

## Contributing

Contributions, issues, and feature requests are welcome. Please open an issue to discuss proposed changes before submitting a pull request.

---

## License

This project is released under the GPL v3 License.
