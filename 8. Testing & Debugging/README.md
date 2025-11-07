### **90. Unit Testing (Mocha, Jest, Supertest)**

- **Unit testing** checks _individual functions or modules_ to ensure they work correctly in isolation.
- Common frameworks:

  - **Mocha** → test runner (flexible, works with Chai for assertions).
  - **Jest** → all-in-one testing framework (assertions + mocks + coverage).
  - **Supertest** → used to test Express APIs (makes HTTP requests to your routes).

**Example (with Jest + Supertest):**

```js
const request = require("supertest");
const app = require("../app");

test("GET /api/users should return 200", async () => {
  const res = await request(app).get("/api/users");
  expect(res.statusCode).toBe(200);
});
```

✅ Unit tests are fast, repeatable, and great for CI pipelines.

---

### **91. Integration Testing**

- Tests **multiple modules together** (e.g., route + controller + DB).
- Goal: ensure all connected parts work properly as a system.
- Example (User signup flow):

  1. Hit signup endpoint.
  2. Verify response.
  3. Check if user is actually stored in DB.

**Example:**

```js
it("should create a new user in DB", async () => {
  const res = await request(app)
    .post("/api/users")
    .send({ name: "Bilal", email: "bilal@test.com" });
  expect(res.status).toBe(201);
  const user = await User.findOne({ email: "bilal@test.com" });
  expect(user).not.toBeNull();
});
```

Integration tests are slower than unit tests but essential before deployment.

---

### **92. Mocking Databases & Functions**

- **Mocking** = creating fake versions of dependencies (like DBs or APIs) for testing.
- Prevents tests from relying on real data or external services.
- Tools:

  - Jest’s built-in `jest.mock()`
  - `sinon` for stubs/spies
  - `mongodb-memory-server` for fake in-memory MongoDB

```js
jest.mock("../models/User", () => ({
  find: jest.fn().mockResolvedValue([{ name: "Bilal" }]),
}));
```

✅ This makes tests predictable, fast, and independent.

---

### **93. API Testing with Postman**

- **Postman** = GUI tool for manually testing APIs.
- You can:

  - Send GET/POST/PUT/DELETE requests.
  - Set headers, body, and authorization.
  - Save and organize requests into collections.
  - Automate testing using **Postman Tests** (JavaScript assertions).

- Example test snippet in Postman:

  ```js
  pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
  });
  ```

---

### **94. Debugging Tools & Techniques**

- Debugging helps identify and fix runtime issues in Node.js.
- Techniques:

  - **Console debugging**: `console.log()`, `console.error()`
  - **Node Inspector**: run with

    ```
    node --inspect app.js
    ```

    Then open `chrome://inspect` in Chrome.

  - **VS Code Debugger**: built-in breakpoints and stepping.
  - **Error tracking tools**:

    - **Sentry**
    - **LogRocket**
    - **Winston** (file logging)

  - Add custom error messages, handle unhandled rejections, and always use descriptive logs.

---

✅ Summary:

| Type        | Tool                | Purpose                        |
| ----------- | ------------------- | ------------------------------ |
| Unit        | Jest / Mocha        | Test single functions          |
| Integration | Jest + Supertest    | Test combined modules          |
| API         | Postman             | Manual endpoint testing        |
| Mocking     | Jest / Sinon        | Simulate external dependencies |
| Debugging   | VS Code / Inspector | Fix runtime issues             |

---
