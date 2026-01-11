---

---

A MySQL model encapsulates all SQL queries related to one database entity (table).

A model in a MySQL application represents a database table and contains reusable SQL queries and data access logic, keeping database operations separate from controllers.

**user.model.js - methods to access data from mysql**

- Encapsulate SQL queries
- Reusable across controllers
- No request/response logic

SQL user Table

`CREATE TABLE users (
id INT PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(100) NOT NULL,
email VARCHAR(100) UNIQUE NOT NULL,
password VARCHAR(255) NOT NULL,
role ENUM('user','admin') DEFAULT 'user',
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);`

`const db = require("../database/MySQL.database");`

`const User = {`

`// Create new user
create: async (user) => {
const [result] = await db.execute(
INSERT INTO users (name, email, password, role)        VALUES (?, ?, ?, ?),
[`[`user.name`](http://user.name/)`, user.email, user.password, user.role || "user"]
);
return result.insertId;
},`

`// Find user by email
findByEmail: async (email) => {
const [rows] = await db.execute(
"SELECT * FROM users WHERE email = ?",
[email]
);
return rows[0];
},`

`// Find user by ID
findById: async (id) => {
const [rows] = await db.execute(
"SELECT id, name, email, role FROM users WHERE id = ?",
[id]
);
return rows[0];
},`

/`/ Update user
update: async (id, data) => {
const [result] = await db.execute(
"UPDATE users SET name = ?, role = ? WHERE id = ?",
[`[`data.name`](http://data.name/)`, data.role, id]
);
return result.affectedRows;
},`

`// Delete user
remove: async (id) => {
const [result] = await db.execute(
"DELETE FROM users WHERE id = ?",
[id]
);
return result.affectedRows;
},
};`

`module.exports = User;`

How to use it (controller)

`const User = require("../models/User.model");
const bcrypt = require("bcrypt");`

`exports.register = async (req, res, next) => {
try {
const hashedPassword = await bcrypt.hash(req.body.password, 10);`

`const userId = await User.create({
name: `[`req.body.name`](http://req.body.name/)`,
email: req.body.email,
password: hashedPassword,
});`

`res.status(201).json({ id: userId });`

`} catch (err) {
next(err);
}
};`