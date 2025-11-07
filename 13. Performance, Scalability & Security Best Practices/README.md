## 🚀 **13. Performance, Scalability & Security Best Practices**

These are the advanced concepts that ensure your backend runs **fast, stable, and secure** — especially when your app grows in traffic or moves to production.

---

### **128. Caching Strategies (Redis, In-memory)**

- Cache frequently requested data to reduce database load.
- **Redis** is the most common caching layer.
  Example:

```js
// pseudo code
const cached = await redis.get("users");
if (cached) return JSON.parse(cached);

const users = await User.find();
await redis.set("users", JSON.stringify(users), "EX", 3600);
```

✅ Improves response time drastically for heavy endpoints.

---

### **129. Load Balancing & Scaling**

- Distribute incoming requests across multiple Node.js instances.
- Use:

  - **PM2 cluster mode** for multi-core scaling.
  - **Nginx or AWS Load Balancer** for distributed scaling.
    ✅ Prevents bottlenecks and increases app availability.

---

### **130. Database Optimization**

- Use **indexes** on frequently queried fields:

  ```js
  userSchema.index({ email: 1 });
  ```

- Avoid unnecessary queries; use **projection** and **pagination**.
- Use **connection pooling** for efficiency.
  ✅ Ensures database queries stay fast and efficient.

---

### **131. API Rate Limiting & Throttling**

- Prevent excessive requests from a single user/IP.
- Commonly done with:

  ```js
  import rateLimit from "express-rate-limit";
  app.use(rateLimit({ windowMs: 15 * 60 * 1000, max: 100 }));
  ```

✅ Protects from brute-force and DDoS attacks.

---

### **132. Secure HTTP Headers (Helmet)**

- Use the **Helmet** middleware to automatically set security headers:

  ```js
  import helmet from "helmet";
  app.use(helmet());
  ```

✅ Protects against common vulnerabilities like clickjacking, XSS, and MIME sniffing.

---

### **133. Input Validation & Sanitization**

- Use `Joi`, `Zod`, or `express-validator` for schema-based input validation.
- Combine with:

  ```js
  import xss from "xss-clean";
  import mongoSanitize from "express-mongo-sanitize";
  app.use(xss());
  app.use(mongoSanitize());
  ```

✅ Prevents SQL/NoSQL injections and XSS attacks.

---

### **134. HTTPS and SSL Setup**

- Always use **HTTPS** in production.
- Free SSL with **Let’s Encrypt** or via **Cloudflare** proxy.
  ✅ Encrypts data in transit and prevents man-in-the-middle attacks.

---

### **135. Logging and Monitoring**

- Use **Winston**, **Morgan**, or external services like **Sentry**, **Datadog**, or **LogRocket**.
- Monitor:

  - API latency
  - Error rates
  - CPU & memory usage
    ✅ Detects issues early and helps maintain uptime.

---

### **136. Graceful Error Handling**

- Wrap async routes with error handlers:

  ```js
  app.use((err, req, res, next) => {
    res.status(err.status || 500).json({ message: err.message });
  });
  ```

✅ Prevents crashes and gives meaningful error responses to clients.

---

### **137. Asynchronous & Non-Blocking Code**

- Use **async/await** and **Promises** instead of blocking operations.
- Heavy computations → move to **worker threads** or **queues**.
  ✅ Keeps the event loop free and response times consistent.

---

### **138. Environment-based Configuration**

- Keep separate configs for `development`, `staging`, and `production`.
- Example:

  ```bash
  NODE_ENV=production
  ```

- Load different `.env` files using packages like `dotenv-flow`.
  ✅ Simplifies deployment and environment management.

---

### **139. Graceful Shutdown**

- On app termination (SIGINT/SIGTERM), close DB connections and pending requests.

```js
process.on("SIGTERM", async () => {
  await mongoose.connection.close();
  server.close(() => console.log("App closed gracefully"));
});
```

✅ Prevents data loss and incomplete transactions on server shutdown.

---

### **140. Secure JWT Practices**

- Use **short expiry** (e.g., 15min) for access tokens.
- Use **refresh tokens** with rotation.
- Store tokens securely (preferably in cookies or HttpOnly if possible).
  ✅ Prevents token theft and unauthorized reuse.

---

✅ **Summary Table**

| Topic                   | Purpose                          |
| ----------------------- | -------------------------------- |
| Caching                 | Faster response times            |
| Load Balancing          | Handle large traffic efficiently |
| DB Optimization         | Reduce query time                |
| Rate Limiting           | Prevent abuse                    |
| Helmet                  | Add secure headers               |
| Validation/Sanitization | Prevent attacks via user input   |
| HTTPS                   | Encrypt communication            |
| Logging/Monitoring      | Track performance & issues       |
| Error Handling          | Avoid crashes                    |
| Async/Non-blocking      | Maintain event loop performance  |
| Env Config              | Manage multiple environments     |
| Graceful Shutdown       | Prevent data corruption          |
| JWT Security            | Protect against token theft      |

---
