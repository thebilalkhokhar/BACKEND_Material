### **48. Introduction to NoSQL & MongoDB**

- **NoSQL** = “Not Only SQL,” a type of database designed for flexible, scalable data storage.
- **MongoDB** is a **document-oriented NoSQL database** that stores data in **JSON-like BSON** format.
- Instead of tables and rows → we use **collections** and **documents**.
- Best for apps with flexible schemas, like user profiles or product catalogs.

---

### **49. MongoDB vs SQL**

| Feature      | MongoDB (NoSQL)                           | SQL (Relational)            |
| ------------ | ----------------------------------------- | --------------------------- |
| Data format  | JSON-like (BSON)                          | Tables (rows & columns)     |
| Schema       | Dynamic / flexible                        | Fixed schema                |
| Joins        | Limited (use references)                  | Supported                   |
| Transactions | Supported (since MongoDB 4.x)             | Fully supported             |
| Scalability  | Horizontal (sharding)                     | Vertical (adding CPU/RAM)   |
| Use case     | Real-time, large-scale, unstructured data | Structured, relational data |

---

### **50. Installing & Connecting MongoDB**

- Install **MongoDB Community Edition** or use **MongoDB Atlas** (cloud-based).
- Connect using the **MongoDB URI**:

  ```js
  mongoose.connect("mongodb://localhost:27017/myapp");
  ```

- Use `.env` file for connection string security.
- Example:

  ```
  MONGO_URI=mongodb+srv://user:pass@cluster.mongodb.net/dbname
  ```

---

### **51. Collections, Documents, Databases**

- **Database** → container of collections.
- **Collection** → similar to a table in SQL.
- **Document** → JSON-like record inside a collection.

  ```json
  {
    "_id": "123",
    "name": "Bilal",
    "email": "bilal@example.com"
  }
  ```

---

### **52. CRUD operations**

CRUD = Create, Read, Update, Delete.
Examples using Mongo shell or Mongoose:

- **Insert** → `db.users.insertOne({ name: 'Bilal' })`
- **Find** → `db.users.find({ name: 'Bilal' })`
- **Update** → `db.users.updateOne({ name: 'Bilal' }, { $set: { age: 24 } })`
- **Delete** → `db.users.deleteOne({ name: 'Bilal' })`

---

### **53. MongoDB Query Operators**

Used for filtering or updating data:

- `$eq`, `$ne`, `$gt`, `$lt`, `$in`, `$nin`, `$and`, `$or`
- Example:

  ```js
  db.users.find({ age: { $gt: 18, $lt: 30 } });
  ```

- Update operators like `$set`, `$inc`, `$push`, `$pull`.

---

### **54. Aggregation Pipeline Basics**

- Used for **data analysis and transformation**.
- Similar to SQL’s `GROUP BY` or joins.
- Stages like `$match`, `$group`, `$sort`, `$limit`, `$project`.

  ```js
  db.orders.aggregate([
    { $match: { status: "delivered" } },
    { $group: { _id: "$userId", total: { $sum: "$amount" } } },
  ]);
  ```

---

### **55. Data Modeling (Embedding vs Referencing)**

- **Embedding** → store related data in the same document.
  (Faster reads, good for small sub-documents)

  ```js
  { name: "Bilal", orders: [{ item: "Pizza", price: 10 }] }
  ```

- **Referencing** → use `ObjectId` reference to another collection.
  (Good for large or reusable data)

  ```js
  { name: "Bilal", orders: [orderId1, orderId2] }
  ```

---

### **56. Schema Design Best Practices**

- Keep documents small and focused.
- Use embedding for one-to-few relationships.
- Use referencing for one-to-many or many-to-many.
- Always index frequently queried fields.
- Avoid deeply nested objects.
- Plan for scalability (use sharding if needed).

---

### **57. Mongoose ODM Introduction**

- **Mongoose** = ODM (Object Data Modeling) library for MongoDB + Node.js.
- It provides a schema-based way to model data and simplify CRUD operations.
- Helps with validation, population, middleware, etc.

---

### **58. Defining Schemas & Models**

- Define structure for each collection:

  ```js
  const userSchema = new mongoose.Schema({
    name: String,
    email: { type: String, required: true, unique: true },
    age: Number,
  });
  const User = mongoose.model("User", userSchema);
  ```

---

### **59. CRUD with Mongoose**

```js
// Create
await User.create({ name: "Bilal", email: "bilal@mail.com" });

// Read
const users = await User.find();

// Update
await User.updateOne({ _id: id }, { $set: { name: "Ali" } });

// Delete
await User.deleteOne({ _id: id });
```

---

### **60. Validation & Custom Validators**

- Define rules in schema:

  ```js
  email: { type: String, required: true, match: /@/ }
  ```

- Custom validation:

  ```js
  age: {
    type: Number,
    validate: (v) => v > 0
  }
  ```

---

### **61. Mongoose Middleware (pre, post hooks)**

- Run code before or after certain operations:

  ```js
  userSchema.pre("save", function (next) {
    console.log("Saving user...");
    next();
  });
  userSchema.post("save", function (doc) {
    console.log("User saved:", doc);
  });
  ```

---

### **62. Virtuals & Getters/Setters**

- **Virtuals** → fields not stored in DB but computed dynamically:

  ```js
  userSchema.virtual("fullName").get(function () {
    return `${this.firstName} ${this.lastName}`;
  });
  ```

- **Getters/Setters** → modify values on get/set.

---

### **63. Population (refs)**

- Used to join related documents (like foreign keys).

  ```js
  const postSchema = new Schema({
    title: String,
    author: { type: Schema.Types.ObjectId, ref: "User" },
  });
  const posts = await Post.find().populate("author");
  ```

- Returns full author details instead of just ID.

---

### **64. Indexing & Performance Optimization**

- Index fields for faster queries:

  ```js
  userSchema.index({ email: 1 });
  ```

- Monitor query performance using `.explain()`.
- Avoid large arrays, and use projection to limit returned fields.

---

### **65. Transactions & Sessions**

- Ensures **atomic operations** (either all succeed or none).
- Example:

  ```js
  const session = await mongoose.startSession();
  session.startTransaction();
  await User.create([{ name: "Bilal" }], { session });
  await Order.create([{ item: "Burger" }], { session });
  await session.commitTransaction();
  ```

---

### **66. Pagination & Filtering**

- Use `.limit()` and `.skip()`:

  ```js
  const page = 2,
    limit = 10;
  const users = await User.find()
    .skip((page - 1) * limit)
    .limit(limit);
  ```

- Filtering example:

  ```js
  const users = await User.find({ age: { $gte: 18 } }).sort("name");
  ```

---
