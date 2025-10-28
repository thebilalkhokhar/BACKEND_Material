## 🧩 **3. Express.js Fundamentals**

---

### **24️⃣ What is Express & Why Use It**

**Express.js** is a **web framework for Node.js** that simplifies building web servers and APIs.
Without Express, you’d manually handle routes, headers, and requests — which is complex.

✅ **Why use Express:**

- Minimal and flexible
- Handles routing easily
- Middleware support
- Great integration with databases, auth, and frontend apps
- Powerful ecosystem (e.g., `morgan`, `helmet`, `cors`)

**Example (Hello World Server):**

```js
import express from "express";
const app = express();

app.get("/", (req, res) => res.send("Hello World"));
app.listen(3000, () => console.log("Server running on port 3000"));
```

---

### **25️⃣ Setting up an Express App**

**Steps:**

1. Install Express

   ```bash
   npm install express
   ```

2. Create `index.js`

   ```js
   import express from "express";
   const app = express();

   app.use(express.json()); // middleware
   app.get("/", (req, res) => res.send("Server is ready"));
   app.listen(5000, () => console.log("Server running at port 5000"));
   ```

**Important:**
Always use middleware before defining routes.

---

### **26️⃣ Middleware Concept**

**Middleware** are functions that run between the request and the response.
They can:

- Modify request or response objects
- End the request-response cycle
- Pass control to the next middleware using `next()`

**Structure:**

```js
app.use((req, res, next) => {
  console.log("Middleware executed!");
  next(); // pass control
});
```

**Types of Middleware:**

- Built-in (from Express)
- Custom (your own)
- Third-party (installed packages)

---

### **27️⃣ Built-in Middleware**

Express comes with a few built-in middlewares:

#### `express.json()`

Parses incoming JSON data (used in APIs).

```js
app.use(express.json());
```

#### `express.urlencoded()`

Parses URL-encoded form data (used in HTML forms).

```js
app.use(express.urlencoded({ extended: true }));
```

#### `express.static()`

Serves static files like images, CSS, JS.

```js
app.use(express.static("public"));
```

---

### **28️⃣ Custom Middleware**

You can write your own middleware for logging, authentication, etc.

**Example:**

```js
const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
};

app.use(logger);
```

You can make them global or route-specific:

```js
app.get("/home", logger, (req, res) => res.send("Home Page"));
```

---

### **29️⃣ Third-Party Middleware (morgan, cors, helmet)**

#### **1. Morgan (HTTP Logger)**

Logs incoming requests — useful for debugging.

```js
import morgan from "morgan";
app.use(morgan("dev"));
```

#### **2. CORS (Cross-Origin Resource Sharing)**

Allows frontend from a different domain to access your API.

```js
import cors from "cors";
app.use(cors());
```

#### **3. Helmet (Security Headers)**

Adds security headers to protect against attacks.

```js
import helmet from "helmet";
app.use(helmet());
```

---

### **30️⃣ Request & Response Objects**

Express provides two key objects in every route:

- **req** → request (data coming in)
- **res** → response (data going out)

**Example:**

```js
app.get("/user/:id", (req, res) => {
  console.log(req.params.id); // path parameter
  console.log(req.query.name); // query string
  console.log(req.body); // body data (POST)
  res.status(200).json({ success: true });
});
```

Common `res` methods:

- `res.send()`
- `res.json()`
- `res.status()`
- `res.redirect()`

---

### **31️⃣ Routing (GET, POST, PUT, DELETE)**

Express makes RESTful routes simple.

**Example:**

```js
app.get("/users", (req, res) => res.send("Get all users"));
app.post("/users", (req, res) => res.send("Create user"));
app.put("/users/:id", (req, res) => res.send("Update user"));
app.delete("/users/:id", (req, res) => res.send("Delete user"));
```

Each route matches an HTTP method and endpoint.

---

### **32️⃣ Route Parameters & Query Strings**

#### **Route Parameters**

Used for dynamic URLs (like `/users/123`).

```js
app.get("/users/:id", (req, res) => {
  res.send(`User ID is ${req.params.id}`);
});
```

#### **Query Strings**

Used for optional filters or parameters (`?name=Bilal&age=23`).

```js
app.get("/search", (req, res) => {
  res.send(req.query); // { name: "Bilal", age: "23" }
});
```

---

### **33️⃣ Route Modularization (express.Router)**

Instead of putting all routes in one file, you can split them using **Routers**.

**Example:**

`routes/userRoutes.js`

```js
import express from "express";
const router = express.Router();

router.get("/", (req, res) => res.send("All users"));
router.post("/", (req, res) => res.send("Create user"));

export default router;
```

`index.js`

```js
import userRoutes from "./routes/userRoutes.js";
app.use("/api/users", userRoutes);
```

Now `/api/users` → handled by `userRoutes.js`.

---

### **34️⃣ Error Handling Middleware**

Special middleware with **4 parameters**:

```js
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: err.message });
});
```

To trigger it manually:

```js
app.get("/error", (req, res, next) => {
  next(new Error("Something went wrong"));
});
```

Always place this **at the end** of all routes.

---

### **35️⃣ Serving Static Files**

Used to serve assets like HTML, CSS, JS, or images.

**Example:**

```
project/
 ├── public/
 │   ├── index.html
 │   ├── style.css
 └── server.js
```

```js
app.use(express.static("public"));
```

Now you can open `http://localhost:5000/index.html`.

---

### **36️⃣ Template Engines (EJS, Pug, Handlebars)**

Template engines let you **render dynamic HTML** on the server.

#### **EJS (Embedded JavaScript):**

```bash
npm install ejs
```

```js
app.set("view engine", "ejs");
app.get("/", (req, res) => res.render("index", { name: "Bilal" }));
```

**index.ejs**

```html
<h1>Hello <%= name %></h1>
```

#### **Pug Example:**

```bash
npm install pug
```

```js
app.set("view engine", "pug");
app.get("/", (req, res) => res.render("index", { title: "Home" }));
```

#### **Handlebars Example:**

```bash
npm install express-handlebars
```

```js
import { engine } from "express-handlebars";
app.engine("handlebars", engine());
app.set("view engine", "handlebars");
```

---

✅ **In summary:**
Express gives you the full toolbox for:

- Handling requests & responses
- Organizing routes cleanly
- Managing middleware
- Adding security & performance layers
- Rendering HTML or serving APIs

---
