**🚀 9. Deployment & DevOps Integration**

### **95. Environment Variables Management (`dotenv`)**

- Used to store **sensitive configuration data** (API keys, DB URIs, secrets).
- Store in `.env` file → load using `dotenv` package.

```js
require("dotenv").config();
const port = process.env.PORT || 5000;
```

✅ Keeps secrets out of your codebase, supports multiple environments (dev/prod/test).

---

### **96. Process Managers (PM2)**

- PM2 keeps your Node app running **24/7** — restarts automatically on crash or reboot.
- Key commands:

  ```bash
  pm2 start server.js --name "myapp"
  pm2 restart myapp
  pm2 logs myapp
  pm2 save
  pm2 startup
  ```

✅ Supports **load balancing**, **logging**, and **clustering**.

---

### **97. Nginx Reverse Proxy**

- Acts as a **gateway** between users and your Node server.
- Commonly used to:

  - Serve HTTPS with SSL.
  - Route traffic to internal ports (e.g., 3000 → 80/443).
  - Handle static file serving & load balancing.

Example Nginx config:

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
    }
}
```

✅ Ensures scalability, better security, and faster response times.

---

### **98. Load Balancing**

- Distributes incoming traffic across multiple app instances.
- In Node.js, can be done via:

  - **PM2 clustering** mode.
  - **Nginx** load balancing.
  - **Cloud provider’s load balancer** (e.g., AWS ELB).

Example Nginx load balancer:

```nginx
upstream node_app {
    server localhost:3001;
    server localhost:3002;
}
```

✅ Improves **availability**, **scalability**, and **fault tolerance**.

---

### **99. CI/CD Pipelines (GitHub Actions, etc.)**

- Automates code testing, building, and deployment.
- **CI (Continuous Integration):** run tests automatically on push.
- **CD (Continuous Deployment):** auto-deploy to production on successful CI.

Example GitHub Actions workflow (`.github/workflows/deploy.yml`):

```yaml
name: Deploy App
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm install
      - run: npm test
```

✅ Saves time, ensures consistency, and reduces human error.

---

### **100. Hosting (Render, Railway, AWS EC2, Vercel)**

- Popular Node.js hosting platforms:

  - **Render** → simple Git-based deployment.
  - **Railway** → quick auto-deployment with DBs.
  - **AWS EC2** → full control, suitable for production.
  - **Vercel** → frontend + serverless backend.

- Typical steps:

  1. Push code to GitHub.
  2. Connect hosting platform.
  3. Set environment variables.
  4. Deploy and test.

✅ Always set your `PORT` and `MONGODB_URI` in environment settings.

---

### **101. Connecting Frontend to Backend (CORS + API Base URL)**

- **CORS (Cross-Origin Resource Sharing)** controls access between domains.
- Use the `cors` package in Express:

  ```js
  import cors from "cors";
  app.use(cors({ origin: "https://yourfrontend.com", credentials: true }));
  ```

- Set API Base URL in frontend (e.g., `https://api.yourapp.com`).

✅ Prevents CORS errors and allows secure data exchange between frontend & backend.

---

### **102. MongoDB Atlas Setup**

- **MongoDB Atlas** is a cloud-hosted MongoDB service.
  Steps:

1. Create an Atlas cluster.
2. Add IP whitelist (`0.0.0.0/0` or specific IP).
3. Create DB user with credentials.
4. Connect using URI:

   ```bash
   MONGODB_URI=mongodb+srv://user:password@cluster.mongodb.net/dbname
   ```

✅ Offers auto-scaling, backups, and monitoring.

---

### **103. SSL / HTTPS Setup**

- SSL encrypts communication between client and server.
- Generate SSL certs (via **Let’s Encrypt** or **Cloudflare**).
- Configure in Nginx:

  ```nginx
  server {
      listen 443 ssl;
      ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
      ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
      proxy_pass http://localhost:3000;
  }
  ```

✅ Ensures **secure data transfer**, required for production.

---

✅ **Summary Table:**

| Topic          | Purpose                                |
| -------------- | -------------------------------------- |
| dotenv         | Manage sensitive environment variables |
| PM2            | Keep Node server running 24/7          |
| Nginx          | Reverse proxy + SSL + load balancing   |
| Load Balancing | Scale across multiple servers          |
| CI/CD          | Automate build & deploy                |
| Hosting        | Deploy backend online                  |
| CORS           | Connect frontend securely              |
| MongoDB Atlas  | Cloud database hosting                 |
| SSL/HTTPS      | Secure client-server communication     |

---
