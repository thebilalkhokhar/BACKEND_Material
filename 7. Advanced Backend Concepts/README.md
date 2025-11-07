## **🧰 7. Advanced Backend Concepts**

### **79. MVC Architecture**

- **MVC (Model–View–Controller)** = a design pattern for structuring backend apps.
- Separates responsibilities to make code cleaner, scalable, and testable.

  - **Model:** Data logic (e.g., MongoDB models with Mongoose).
  - **View:** What user sees (in APIs, usually JSON responses instead of HTML).
  - **Controller:** Handles incoming requests, calls models, and sends responses.

- Folder structure example:

  ```
  ├── models/
  │   └── user.model.js
  ├── controllers/
  │   └── user.controller.js
  ├── routes/
  │   └── user.routes.js
  ├── app.js
  └── server.js
  ```

---

### **80. Dependency Injection Concept**

- **Dependency Injection (DI)** means passing required dependencies (services, DBs, config) into a class or function instead of hardcoding them.
- Benefits:

  - Easier testing (mock dependencies)
  - Cleaner and more flexible code

- Example:

  ```js
  class UserService {
    constructor(userRepo) {
      this.userRepo = userRepo; // Injected dependency
    }
    createUser(data) {
      return this.userRepo.save(data);
    }
  }
  ```

---

### **81. Caching (Redis, Node Cache)**

- Caching stores frequently accessed data temporarily for faster responses.
- **Redis** = in-memory data store (super fast).
- Example (with Redis):

  ```js
  const redis = require("redis");
  const client = redis.createClient();

  app.get("/users", async (req, res) => {
    const cache = await client.get("users");
    if (cache) return res.json(JSON.parse(cache));

    const users = await User.find();
    await client.set("users", JSON.stringify(users), "EX", 60);
    res.json(users);
  });
  ```

- **Node-cache** → simple local caching solution for small apps.

---

### **82. Background Jobs (Bull, Agenda, Node-cron)**

Used to offload heavy or time-consuming tasks (email, reports, notifications):

- **Bull** → job queue built on Redis.
- **Agenda** → MongoDB-based job scheduler.
- **Node-cron** → run tasks periodically (like cron jobs).

  ```js
  const cron = require("node-cron");
  cron.schedule("0 0 * * *", () => console.log("Runs daily at midnight"));
  ```

- Benefits:

  - Keeps main API fast.
  - Handles retries, delays, and job queues.

---

### **83. WebSockets / Socket.io (Real-Time Communication)**

- WebSockets enable **two-way, real-time communication** between client & server.
- **Socket.io** simplifies WebSocket implementation.
- Example:

  ```js
  const io = require("socket.io")(server);
  io.on("connection", (socket) => {
    console.log("User connected");
    socket.emit("welcome", "Hello Client!");
    socket.on("message", (data) => console.log(data));
  });
  ```

- Used in: live chat, notifications, real-time dashboards, multiplayer games.

---

### **84. API Versioning**

- Allows you to update APIs without breaking existing clients.
- Common methods:

  - URL versioning → `/api/v1/users`
  - Header versioning → `Accept: application/vnd.myapp.v2+json`

- Keep old routes active while new ones are deployed.

  ```
  /routes/v1/userRoutes.js
  /routes/v2/userRoutes.js
  ```

---

### **85. Request Throttling & Concurrency Control**

- Prevents system overload by limiting concurrent or frequent requests.
- Tools:

  - `express-rate-limit` (basic rate limiting)
  - Custom logic for concurrency:

    ```js
    let activeRequests = 0;
    app.use((req, res, next) => {
      if (activeRequests >= 100) return res.status(503).send("Server busy");
      activeRequests++;
      res.on("finish", () => activeRequests--);
      next();
    });
    ```

---

### **86. Environment Configuration per Stage (dev, prod)**

- Maintain different settings for development, testing, and production:

  ```
  .env.development
  .env.production
  ```

- Load with `dotenv`:

  ```js
  require("dotenv").config({ path: `.env.${process.env.NODE_ENV}` });
  ```

- Example:

  ```
  NODE_ENV=production
  PORT=8080
  DB_URI=prod-db-uri
  ```

---

### **87. Performance Optimization Techniques**

Key optimizations for Node.js backends:

1. Use **async/await** and avoid blocking code.
2. Use **caching** (Redis, CDN).
3. Optimize **database queries** (indexes, projection).
4. Compress responses (use `compression` middleware).
5. Use **pagination** instead of loading large data.
6. Deploy with **load balancers** (NGINX).
7. Profile performance with `clinic.js` or `pm2`.

---

### **88. Clustering in Node.js**

- Node.js runs on a **single thread**, but clustering allows usage of multiple CPU cores.
- Built-in **cluster module** helps scale the app:

  ```js
  const cluster = require("cluster");
  const os = require("os");

  if (cluster.isPrimary) {
    os.cpus().forEach(() => cluster.fork());
  } else {
    require("./server");
  }
  ```

- Great for performance in high-traffic production apps.

---

### **89. Worker Threads for CPU-Intensive Tasks**

- For heavy computations (e.g., encryption, image processing), use **worker threads** so they don’t block the main event loop.

  ```js
  const { Worker } = require("worker_threads");
  const worker = new Worker("./heavyTask.js");
  worker.postMessage({ input: 10 });
  worker.on("message", (result) => console.log(result));
  ```

- Helps maintain responsiveness even under CPU load.

---
