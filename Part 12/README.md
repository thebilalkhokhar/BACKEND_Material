**🧩 12. Integration Topics** — these connect your **backend (Node/Express/Mongo)** with your **frontend (React)** for a complete full-stack system:

---

### **122. REST API Integration with React Frontend**

- The frontend communicates with backend APIs using **HTTP requests (GET, POST, PUT, DELETE)**.
- Commonly done via **Axios** or **Fetch API**.
  Example:

```js
// frontend: api.js
import axios from "axios";

export const api = axios.create({
  baseURL: "https://api.yourapp.com",
});
```

- React components then call:

```js
const res = await api.get("/users");
setUsers(res.data);
```

✅ Allows dynamic data fetching and seamless connection between client and server.

---

### **123. Pagination, Filtering, Sorting APIs**

- Backend APIs should support query parameters for efficient data retrieval.
  Example backend route:

```js
GET /products?page=2&limit=10&sort=price&category=shoes
```

Backend logic:

```js
const { page = 1, limit = 10, sort = "createdAt" } = req.query;
const products = await Product.find()
  .sort(sort)
  .skip((page - 1) * limit)
  .limit(limit);
```

✅ Improves performance, especially for large datasets.

---

### **124. Handling JWT in LocalStorage**

- When user logs in, backend returns a **JWT token**.
- Frontend stores it in:

  ```js
  localStorage.setItem("token", response.data.token);
  ```

- For subsequent requests:

  ```js
  api.defaults.headers.common["Authorization"] = `Bearer ${token}`;
  ```

✅ Allows persistent user sessions without storing credentials.

> ⚠️ **Note:** For higher security, consider **HttpOnly cookies** to protect against XSS — but `localStorage` is common for SPA setups.

---

### **125. Protecting Routes on Frontend**

- Certain React routes (like `/dashboard`) should only be accessible to logged-in users.
  Example using **React Router**:

```js
const PrivateRoute = ({ children }) => {
  const token = localStorage.getItem("token");
  return token ? children : <Navigate to="/login" />;
};
```

Usage:

```js
<Route
  path="/dashboard"
  element={
    <PrivateRoute>
      <Dashboard />
    </PrivateRoute>
  }
/>
```

✅ Ensures unauthorized users can’t access private pages.

---

### **126. Using Axios Interceptors for Token Refresh**

- Interceptors automatically attach tokens and handle expiry.
  Example:

```js
api.interceptors.request.use((config) => {
  const token = localStorage.getItem("token");
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

api.interceptors.response.use(
  (res) => res,
  async (err) => {
    if (err.response.status === 401) {
      // handle token refresh logic here
    }
    return Promise.reject(err);
  }
);
```

✅ Automates token handling and improves user experience without forcing frequent logins.

---

### **127. Uploading Images from Frontend to Backend**

- Use **Multer** on backend to handle file uploads.
- On frontend:

  ```js
  const formData = new FormData();
  formData.append("image", file);
  await api.post("/upload", formData, {
    headers: { "Content-Type": "multipart/form-data" },
  });
  ```

- Backend:

  ```js
  app.post("/upload", upload.single("image"), (req, res) => {
    res.json({ imageUrl: req.file.path });
  });
  ```

✅ Enables users to upload avatars, product images, etc., directly from UI.

---

✅ **Summary Table**

| Topic                        | Purpose                               |
| ---------------------------- | ------------------------------------- |
| REST API Integration         | Connect frontend to backend endpoints |
| Pagination/Filtering/Sorting | Efficient data retrieval              |
| JWT in LocalStorage          | Maintain authenticated sessions       |
| Frontend Route Protection    | Restrict private pages                |
| Axios Interceptors           | Auto-manage tokens & refresh          |
| Image Upload                 | Send files from client to server      |

---
