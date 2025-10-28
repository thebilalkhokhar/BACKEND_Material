**🧱 11. Backend Design Patterns & Architecture** — these are the **blueprints** that help you build **scalable, maintainable, and professional-grade backend systems**:

---

### **114. Layered Architecture (Controller, Service, Repository)**

A clean separation of concerns — the backbone of most backend systems.

**Layers:**

1. **Controller Layer:** Handles HTTP requests/responses.

   - Example: `userController.js`

2. **Service Layer:** Contains business logic.

   - Example: `userService.js`

3. **Repository/Data Layer:** Handles direct database operations.

   - Example: `userRepository.js`

**Flow:**
`Route → Controller → Service → Repository → Database`

✅ Benefits: Cleaner code, easier testing, and independent maintenance.

---

### **115. Factory & Singleton Pattern in Node.js**

**Factory Pattern:**
Used when you need to create objects dynamically without exposing the creation logic.
Example:

```js
class NotificationFactory {
  static create(type) {
    if (type === "email") return new EmailService();
    if (type === "sms") return new SMSService();
  }
}
```

✅ Great for flexible object creation and decoupling logic.

**Singleton Pattern:**
Ensures **only one instance** of a class exists (e.g., DB connection).
Example:

```js
class Database {
  constructor() {
    if (Database.instance) return Database.instance;
    Database.instance = this;
  }
}
```

✅ Prevents multiple DB connections and improves performance.

---

### **116. Observer Pattern (EventEmitter Usage)**

Used when one part of your app needs to **react to events** triggered by another part.
Node.js’s `EventEmitter` implements this.

Example:

```js
import { EventEmitter } from "events";
const emitter = new EventEmitter();

emitter.on("userRegistered", (data) =>
  console.log("Email sent to", data.email)
);
emitter.emit("userRegistered", { email: "test@gmail.com" });
```

✅ Decouples modules and supports **event-driven programming**.

---

### **117. Repository Pattern with Mongoose**

Abstracts the database logic into a dedicated repository class.

Example:

```js
class UserRepository {
  async createUser(data) {
    return await UserModel.create(data);
  }
  async findUserByEmail(email) {
    return await UserModel.findOne({ email });
  }
}
```

✅ Keeps business logic independent of DB implementation → easier to switch databases (MongoDB → PostgreSQL) later.

---

### **118. Clean Architecture Concepts**

A structured design philosophy focusing on **separation of concerns**:

**Layers (from inner to outer):**

1. **Entities (Core business logic)**
2. **Use Cases / Services**
3. **Interface Adapters (Controllers, Repositories)**
4. **Frameworks / External tools (Express, DB, etc.)**

✅ Makes the system **framework-agnostic**, **testable**, and **scalable**.

---

### **119. Microservices Basics**

Instead of one big backend (monolith), split app into **independent small services** — each handles one feature (auth, payments, orders, etc.).

Each service has:

- Its own database.
- Its own deployment & scaling.
- Communication via **HTTP APIs** or **message queues**.

✅ Improves scalability, fault isolation, and deployment speed.

---

### **120. Message Queues (RabbitMQ, Kafka)**

Used for **asynchronous communication** between microservices.
Example:
When a user signs up → Auth service emits a “userRegistered” event → Email service consumes it and sends a welcome mail.

Tools:

- **RabbitMQ** – simple queue-based messaging.
- **Kafka** – distributed event streaming platform.

✅ Improves performance and decouples services.

---

### **121. Event-Driven Architecture**

System behavior is driven by **events** rather than direct calls.
Example:

- **Event Producer:** Auth service emits “UserCreated”.
- **Event Consumer:** Analytics service listens and stores stats.

Tools: EventEmitter (small scale), Kafka (large scale).

✅ Enables real-time updates, scalability, and loose coupling between components.

---

✅ **Summary Table**

| Concept                   | Purpose                                               |
| ------------------------- | ----------------------------------------------------- |
| Layered Architecture      | Structured separation (Controller-Service-Repository) |
| Factory Pattern           | Flexible object creation                              |
| Singleton Pattern         | One global instance (DB connection)                   |
| Observer Pattern          | Event-based communication                             |
| Repository Pattern        | Abstract database operations                          |
| Clean Architecture        | Framework-agnostic, scalable structure                |
| Microservices             | Decentralized, independent services                   |
| Message Queues            | Async communication between services                  |
| Event-Driven Architecture | Real-time, event-based system behavior                |

---
