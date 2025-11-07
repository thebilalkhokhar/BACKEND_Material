## ⚙️ **2. Node.js Core Concepts**

---

### **12️⃣ What is Node.js & How It Works (V8 engine, single-threaded, event-driven)**

**Node.js** is a **runtime environment** that allows you to run JavaScript **outside the browser** — mainly on the server side.

- It uses **Google’s V8 Engine** (the same engine used in Chrome) to convert JavaScript into machine code.
- Node.js is **single-threaded** (it executes JavaScript code on one thread) but handles **asynchronous tasks** efficiently using **non-blocking I/O**.
- It uses an **event-driven architecture**, meaning it responds to events (like incoming requests, file reads, etc.) using **callbacks** or **Promises** without blocking the main thread.

🧠 **In short:**
Node.js handles many concurrent connections using just one main thread — perfect for scalable applications like APIs and chat servers.

---

### **13️⃣ Node.js Architecture (libuv, Event Loop Phases)**

**Node.js Architecture** consists of:

- **V8 Engine:** Executes JS code.
- **Libuv:** Handles asynchronous I/O operations (thread pool, event loop).
- **Event Loop:** Continuously checks and processes events/callbacks.
- **C++ bindings:** Interface between JS and OS features (file system, network).

**Event Loop Phases:**

1. **Timers** → executes `setTimeout()` and `setInterval()`
2. **Pending Callbacks**
3. **Idle, Prepare**
4. **Poll** → handles new I/O events
5. **Check** → executes `setImmediate()`
6. **Close callbacks**

This loop keeps running as long as there are tasks pending.

🧩 **Example:**

```js
setTimeout(() => console.log("Timer"), 0);
setImmediate(() => console.log("Immediate"));
console.log("Main");

/// Output:
/// Main
/// Timer OR Immediate (depends on system)
```

---

### **14️⃣ REPL, NPM & package.json**

- **REPL (Read-Eval-Print Loop)**: Node’s interactive shell.
  Run `node` in terminal → type JS code → executes instantly.

- **NPM (Node Package Manager)**: Tool for installing and managing libraries.
  Example: `npm install express`

- **package.json**: A file that stores project info, dependencies, scripts, and version.
  Created with: `npm init -y`

**Example:**

```json
{
  "name": "myapp",
  "version": "1.0.0",
  "dependencies": {
    "express": "^4.19.2"
  }
}
```

---

### **15️⃣ File System (fs module)**

Used for reading, writing, deleting, or updating files.

**Example:**

```js
const fs = require("fs");

// Write
fs.writeFileSync("data.txt", "Hello Bilal!");

// Read
const data = fs.readFileSync("data.txt", "utf8");
console.log(data);

// Append
fs.appendFileSync("data.txt", "\nNew Line Added!");

// Delete
fs.unlinkSync("data.txt");
```

For non-blocking performance, prefer **asynchronous methods** like `fs.readFile()` instead of `fs.readFileSync()`.

---

### **16️⃣ OS, Path, and URL modules**

#### **OS Module**

Gives information about the operating system.

```js
const os = require("os");
console.log(os.platform()); // win32
console.log(os.totalmem()); // total memory
```

#### **Path Module**

Handles file paths safely across operating systems.

```js
const path = require("path");
console.log(path.join(__dirname, "folder", "file.txt"));
console.log(path.extname("index.html")); // .html
```

#### **URL Module**

Parses and formats URLs.

```js
const url = require("url");
const myUrl = new URL("https://example.com/products?id=10&cat=books");
console.log(myUrl.hostname); // example.com
console.log(myUrl.searchParams.get("id")); // 10
```

---

### **17️⃣ Events & EventEmitter**

Node.js follows an **event-driven architecture** — actions trigger events handled by listeners.

**Example:**

```js
const EventEmitter = require("events");
const emitter = new EventEmitter();

// Listener
emitter.on("greet", (name) => console.log(`Hello ${name}`));

// Emit event
emitter.emit("greet", "Bilal");
```

Useful in real-time systems like chat apps, sockets, and streaming.

---

### **18️⃣ Streams (Readable, Writable, Duplex, Transform)**

**Streams** handle data **in chunks** — perfect for reading/writing large files efficiently.

- **Readable Stream:** Read data (e.g., fs.createReadStream)
- **Writable Stream:** Write data (e.g., fs.createWriteStream)
- **Duplex:** Both readable & writable (e.g., TCP sockets)
- **Transform:** Modify data during streaming (e.g., compression)

**Example:**

```js
const fs = require("fs");

const readable = fs.createReadStream("input.txt");
const writable = fs.createWriteStream("output.txt");

readable.pipe(writable); // send data chunk by chunk
```

---

### **19️⃣ Buffers**

**Buffers** are temporary memory spaces for handling **binary data** (images, videos, files).

**Example:**

```js
const buffer = Buffer.from("Hello");
console.log(buffer); // <Buffer 48 65 6c 6c 6f>
console.log(buffer.toString()); // Hello
```

Buffers are used when data comes in chunks — like streaming or file uploads.

---

### **20️⃣ Global Objects & Process Object**

**Global objects** in Node.js include:

- `__dirname` → current directory path
- `__filename` → current file path
- `require()` → import module
- `module` → current module info
- `exports` → export functionality

**Process Object:**
Gives access to the current Node.js process (running program).

**Example:**

```js
console.log(process.pid); // process id
console.log(process.env.NODE_ENV); // environment variable
```

---

### **21️⃣ Child Processes & Worker Threads**

Used to **run multiple tasks concurrently** or **perform CPU-intensive work** without blocking the main thread.

#### **Child Process Example:**

```js
const { exec } = require("child_process");

exec("dir", (error, stdout, stderr) => {
  if (error) console.error(error);
  else console.log(stdout);
});
```

#### **Worker Threads Example:**

```js
const { Worker } = require("worker_threads");

new Worker("./worker.js", { workerData: { value: 5 } });
```

These help distribute work for better performance.

---

### **22️⃣ Environment Variables (dotenv)**

Used to store sensitive or environment-specific data (like DB URL, JWT secret) outside your code.

**Setup:**

1. Install: `npm install dotenv`
2. Create `.env` file:

   ```
   PORT=5000
   DB_URL=mongodb://localhost:27017/myapp
   JWT_SECRET=mysecret
   ```

3. Use it:

   ```js
   import dotenv from "dotenv";
   dotenv.config();

   console.log(process.env.PORT);
   ```

---

### **23️⃣ Debugging and Logging**

**Debugging:**

- Use `console.log()` for quick debugging.
- Use VS Code’s built-in debugger (`node --inspect index.js`).

**Logging:**

- Use libraries like **Winston** or **Morgan** for structured logs.

**Example:**

```js
import morgan from "morgan";
app.use(morgan("dev"));
```

**Winston example:**

```js
import winston from "winston";
const logger = winston.createLogger({
  transports: [new winston.transports.Console()],
});
logger.info("Server started!");
```

---
