### 🔐 4. Authentication & Authorization

Authentication and Authorization are crucial for securing your backend — **Authentication** means verifying _who_ a user is, while **Authorization** decides _what_ that user can do.

---

### **37. Introduction to Auth**

- **Authentication** = verifying user identity (login, signup, token, etc.)
- **Authorization** = granting or denying access based on roles or permissions.
- Example: You log in → Authenticated ✅
  You try to access admin panel → Checked for Authorization 🛡️

---

### **38. Sessions vs Tokens**

- **Sessions**:

  - Server stores user session data in memory or DB.
  - Client gets a cookie with session ID.
  - Good for traditional web apps.
  - Harder to scale across servers.

- **Tokens (JWT)**:

  - Client stores token (usually in `localStorage` or `cookies`).
  - No session data stored on server (stateless).
  - Better for APIs and microservices.
  - Easier for mobile apps and scalability.

---

### **39. JWT (JSON Web Tokens)**

- A compact, self-contained token used for secure communication.
- Contains:

  - **Header** → algorithm & token type
  - **Payload** → user info (non-sensitive)
  - **Signature** → verifies token integrity

- Created using libraries like `jsonwebtoken`.
- Example:

  ```js
  const token = jwt.sign({ id: user._id }, process.env.JWT_SECRET, {
    expiresIn: "1d",
  });
  const decoded = jwt.verify(token, process.env.JWT_SECRET);
  ```

---

### **40. Password hashing with bcrypt**

- Never store plain passwords.
- **bcrypt** hashes passwords before saving in DB.
- Example:

  ```js
  const hashed = await bcrypt.hash(password, 10);
  const match = await bcrypt.compare(enteredPassword, hashed);
  ```

- Hashing ensures that even if DB leaks, passwords are unreadable.

---

### **41. Login, Signup, Logout flows**

- **Signup**: Validate → Hash password → Save user → Send token.
- **Login**: Find user → Compare password → Return JWT.
- **Logout**:

  - For token-based: simply delete token client-side.
  - For session-based: destroy session on server.

---

### **42. Role-based access control (RBAC)**

- Different users = different permissions.
- Example: Admins can delete users, normal users cannot.
- Implemented by adding `role` field in user model.
- Middleware checks user’s role before executing route:

  ```js
  if (user.role !== "admin") return res.status(403).json("Access Denied");
  ```

---

### **43. Protecting routes & middleware auth check**

- Middleware verifies the JWT before accessing protected routes.

  ```js
  const auth = (req, res, next) => {
    const token = req.headers.authorization?.split(" ")[1];
    if (!token) return res.status(401).send("No token");
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  };
  ```

- Add this middleware to secure APIs.

---

### **44. Refresh tokens & token expiry**

- **Access Token** → short-lived (e.g., 15 mins)
- **Refresh Token** → long-lived (e.g., 7 days)
- Used to issue new access tokens without re-login.
- Refresh tokens are stored securely (often in HttpOnly cookies).

---

### **45. Email verification flow**

- When a user signs up:

  1. Generate a verification token.
  2. Send verification link via email using **nodemailer**.
  3. When the user clicks link → verify token → mark user as verified.

- Prevents fake or duplicate accounts.

---

### **46. Password reset via email (with nodemailer)**

- User requests password reset → backend sends token link.
- Example flow:

  1. User submits email.
  2. Generate password reset token.
  3. Send token via email.
  4. User clicks link → submit new password.
  5. Backend verifies token and updates password.

---

### **47. OAuth & Social login basics (Google, GitHub)**

- Let users sign in using third-party services.
- Uses **OAuth 2.0** protocol.
- Common tools: `passport.js` with strategies like:

  - `passport-google-oauth20`
  - `passport-github`

- Flow:

  1. Redirect user to provider (Google).
  2. Provider asks for permission.
  3. Provider returns access token.
  4. Backend uses token to get user info and log them in.

---
