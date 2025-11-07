**🌐 6. API Development**

### **67. REST API Fundamentals**

- **REST (Representational State Transfer)** = a standard architectural style for designing APIs.
- Works over **HTTP** and uses **stateless** requests.
- Every resource (user, post, order, etc.) has its own **endpoint (URL)**.
- Uses standard HTTP methods:

  - `GET` → Read
  - `POST` → Create
  - `PUT/PATCH` → Update
  - `DELETE` → Delete

---

### **68. RESTful Conventions**

- Follow consistent naming and structure:

  | Action         | Method    | Example Endpoint |
  | -------------- | --------- | ---------------- |
  | Get all users  | GET       | `/api/users`     |
  | Get user by ID | GET       | `/api/users/:id` |
  | Create user    | POST      | `/api/users`     |
  | Update user    | PUT/PATCH | `/api/users/:id` |
  | Delete user    | DELETE    | `/api/users/:id` |

- **Best practices**:

  - Use **nouns**, not verbs (`/users` ✅, `/getUsers` ❌).
  - Return JSON.
  - Use proper HTTP status codes.

---

### **69. CRUD API Endpoints**

Example with Express:

```js
const express = require("express");
const router = express.Router();
const User = require("../models/User");

router.get("/", async (req, res) => res.json(await User.find()));
router.post("/", async (req, res) =>
  res.status(201).json(await User.create(req.body))
);
router.put("/:id", async (req, res) =>
  res.json(await User.findByIdAndUpdate(req.params.id, req.body, { new: true }))
);
router.delete("/:id", async (req, res) =>
  res.json(await User.findByIdAndDelete(req.params.id))
);
```

Each route corresponds to a CRUD operation.

---

### **70. Request Validation (Joi, Zod, express-validator)**

- Validation ensures incoming data is correct and secure.
- Example using **Joi**:

  ```js
  const Joi = require("joi");
  const schema = Joi.object({
    email: Joi.string().email().required(),
    password: Joi.string().min(6).required(),
  });
  const { error } = schema.validate(req.body);
  if (error) return res.status(400).send(error.details[0].message);
  ```

- **Zod** and **express-validator** work similarly but with different syntax.

---

### **71. Response Structure (status codes, messages)**

Always send a **consistent response format**:

```js
res.status(200).json({
  success: true,
  message: "User fetched successfully",
  data: user,
});
```

Common status codes:

| Code | Meaning      |
| ---- | ------------ |
| 200  | OK           |
| 201  | Created      |
| 400  | Bad Request  |
| 401  | Unauthorized |
| 403  | Forbidden    |
| 404  | Not Found    |
| 500  | Server Error |

---

### **72. Global Error Handling Pattern**

Use centralized error middleware:

```js
app.use((err, req, res, next) => {
  console.error(err);
  res.status(err.status || 500).json({
    success: false,
    message: err.message || "Server Error",
  });
});
```

Throw errors using:

```js
throw new Error("Invalid credentials");
```

---

### **73. Async/Await with Try-Catch Wrapper**

Avoid repeating try/catch blocks with a reusable wrapper:

```js
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Example usage
router.get(
  "/",
  asyncHandler(async (req, res) => {
    const users = await User.find();
    res.json(users);
  })
);
```

This prevents unhandled promise rejections and keeps routes clean.

---

### **74. File Uploads (multer, Cloudinary/S3)**

- **Multer** → handles multipart/form-data for local uploads.

  ```js
  const multer = require("multer");
  const upload = multer({ dest: "uploads/" });
  app.post("/upload", upload.single("file"), (req, res) => {
    res.send("File uploaded!");
  });
  ```

- **Cloudinary** or **AWS S3** → for cloud uploads.
  Typically used for profile pictures, documents, etc.

---

### **75. Rate Limiting (express-rate-limit)**

Prevents API abuse or DDoS attacks by limiting request rate:

```js
const rateLimit = require("express-rate-limit");
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100, // limit each IP to 100 requests per 15 minutes
});
app.use(limiter);
```

---

### **76. CORS Setup**

CORS (Cross-Origin Resource Sharing) allows frontend apps on other domains to access your API.

```js
const cors = require("cors");
app.use(cors({ origin: "http://localhost:3000", credentials: true }));
```

Without CORS, browsers block cross-domain requests by default.

---

### **77. Helmet and Security Headers**

- **Helmet** adds various HTTP headers to secure your API.
- Prevents XSS, clickjacking, MIME sniffing, etc.

```js
const helmet = require("helmet");
app.use(helmet());
```

---

### **78. Logging Requests (morgan, Winston)**

- **Morgan** → simple HTTP request logger.

  ```js
  const morgan = require("morgan");
  app.use(morgan("dev"));
  ```

- **Winston** → advanced logger (for saving logs to files or services).

  ```js
  const winston = require("winston");
  const logger = winston.createLogger({
    transports: [new winston.transports.File({ filename: "app.log" })],
  });
  logger.info("Server started...");
  ```

---
