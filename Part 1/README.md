## 🟢 **1. JavaScript Fundamentals (for Backend)**

---

### **1️⃣ ES6+ Syntax (let/const, arrow functions, template literals)**

**ES6 (ECMAScript 2015)** introduced modern syntax and features that make your code more readable and maintainable.

- **let** → block-scoped variable (changes allowed)
- **const** → block-scoped constant (cannot be reassigned)
- **arrow functions** → shorter function syntax, automatically bind `this`
- **template literals** → use backticks for string interpolation

**Example:**

```js
const name = "Bilal";
let age = 23;
console.log(`My name is ${name} and I am ${age} years old.`);
```

---

### **2️⃣ Destructuring & Spread/Rest Operators**

- **Destructuring**: Extract values from arrays or objects easily.
- **Spread (`...`)**: Expands an array/object.
- **Rest (`...`)**: Collects remaining values into an array.

**Example:**

```js
const user = { name: "Bilal", age: 23, city: "Lahore" };
const { name, city } = user; // destructuring
console.log(name, city); // Bilal Lahore

const arr = [1, 2, 3];
const newArr = [...arr, 4, 5]; // spread
console.log(newArr); // [1, 2, 3, 4, 5]

function sum(...nums) {
  // rest
  return nums.reduce((a, b) => a + b);
}
console.log(sum(1, 2, 3, 4)); // 10
```

---

### **3️⃣ Modules (CommonJS vs ES Modules)**

Modules allow you to **split your code** into reusable files.

- **CommonJS** (default in Node.js): uses `require` and `module.exports`
- **ES Modules (ESM)**: uses `import` and `export`

**Example (CommonJS):**

```js
// add.js
module.exports = function (a, b) {
  return a + b;
};

// app.js
const add = require("./add");
console.log(add(2, 3));
```

**Example (ESM):**

```js
// add.js
export function add(a, b) {
  return a + b;
}

// app.js
import { add } from "./add.js";
console.log(add(2, 3));
```

---

### **4️⃣ Promises, async/await**

- **Promise** represents a value that will be available _later_ (async operation).
- **async/await** makes handling Promises easier and cleaner.

**Example with Promise:**

```js
const fetchData = new Promise((resolve, reject) => {
  setTimeout(() => resolve("Data loaded"), 1000);
});

fetchData.then((data) => console.log(data));
```

**Example with async/await:**

```js
async function loadData() {
  const data = await fetchData;
  console.log(data);
}
loadData();
```

---

### **5️⃣ Callbacks & Event Loop**

- **Callback**: a function passed to another function to be executed later.
- **Event Loop**: handles async operations (non-blocking I/O).

**Example:**

```js
function greet(name, callback) {
  console.log("Hello " + name);
  callback();
}

greet("Bilal", () => console.log("Welcome!"));
```

**Event Loop Concept:**
Node.js executes code line by line (single thread) but can handle multiple async tasks via the **event loop** (using the callback queue & microtask queue).

---

### **6️⃣ Error Handling (`try...catch`, `throw`)**

Used to **catch and handle errors** in your code.

**Example:**

```js
try {
  let result = riskyOperation();
  console.log(result);
} catch (error) {
  console.error("Something went wrong:", error.message);
}
```

**Throw custom error:**

```js
function divide(a, b) {
  if (b === 0) throw new Error("Cannot divide by zero");
  return a / b;
}
```

---

### **7️⃣ Classes & Prototypes**

JavaScript is **prototype-based**, but ES6 introduced **classes** as a cleaner syntax for creating objects and inheritance.

**Example:**

```js
class User {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  greet() {
    console.log(`Hi, I’m ${this.name}`);
  }
}

const user = new User("Bilal", 23);
user.greet();
```

Behind the scenes, classes still use **prototypes** for inheritance.

---

### **8️⃣ JSON & Serialization**

- **JSON (JavaScript Object Notation)**: used for data exchange between client and server.
- **Serialization**: converting data/object → string (and vice versa).

**Example:**

```js
const obj = { name: "Bilal", age: 23 };

// Serialize (object → JSON string)
const jsonString = JSON.stringify(obj);

// Deserialize (JSON string → object)
const newObj = JSON.parse(jsonString);
```

Used heavily when sending/receiving data via APIs.

---

### **9️⃣ Map, Filter, Reduce**

Powerful array methods used in data transformation.

**Example:**

```js
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map((n) => n * 2); // [2, 4, 6, 8, 10]
const evens = numbers.filter((n) => n % 2 === 0); // [2, 4]
const sum = numbers.reduce((acc, n) => acc + n, 0); // 15
```

---

### **🔟 Understanding `this` keyword**

`this` refers to the **current execution context** — the object that owns the function.

**Example:**

```js
const user = {
  name: "Bilal",
  showName() {
    console.log(this.name);
  },
};
user.showName(); // Bilal
```

Arrow functions do **not** have their own `this`; they use the `this` of the parent scope.

---

### **1️⃣1️⃣ Closures and Scope**

- **Scope**: defines where variables are accessible.
- **Closure**: a function that _remembers_ variables from its outer scope even after the outer function has finished.

**Example:**

```js
function outer() {
  let count = 0;
  return function inner() {
    count++;
    console.log(count);
  };
}

const counter = outer();
counter(); // 1
counter(); // 2
```

Even though `outer()` finished, `inner()` still accesses `count` — that’s a **closure**.

---
