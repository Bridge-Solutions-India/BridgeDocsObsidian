---

---

	A controller is responsible for processing client requests, invoking the appropriate business or data logic, and sending the HTTP response back to the client.


It acts as the **bridge between routes and business/data logic**.

`Route  →  Controller  →  Model / Service  →  Controller  →  Response`

### Role of Controllers in Your Architecture

==`Controllers:`==
- Receive `req` and `res`
- Extract input (params, body, query)
- Call models/services
- Handle success & error responses
- Do **not** define routes
- Do **not** connect to DB directly

==`Structure`==
controllers/
├── App.controller.js
├── User.controller.js`

==`user.controller.js - handle all user related requests`==
```
const User = require("../models/User.model");
const bcrypt = require("bcrypt");
const { generateToken } = require("../utils/token.util");

exports.register = async (req, res, next) => {
try {
const { name, email, password } = req.body;

const hashedPassword = await bcrypt.hash(password, 10);

const userId = await User.create({
name,
email,
password: hashedPassword,
});

res.status(201).json({
success: true,
userId,
});

} catch (err) {
next(err);
}
};

exports.login = async (req, res, next) => {
try {
const { email, password } = req.body;

const user = await User.findByEmail(email);
if (!user) {
return res.status(404).json({ message: "User not found" });
}

const isValid = await bcrypt.compare(password, user.password);
if (!isValid) {
return res.status(401).json({ message: "Invalid credentials" });
}

const token = generateToken({ id: `[`user.id`](http://user.id/)` });`

res.json({ token });

} catch (err) {
next(err);
}
};
```

==`How to use it (routers)`==
```

const router = require("express").Router();
const UserController = require("../controllers/User.controller");

router.post("/register", UserController.register);
router.post("/login", UserController.login);

module.exports = router;

Error handling in controller (use errorHandler.middleware)

exports.getUser = async (req, res, next) => {
try {
const user = await User.findById(`[`req.params.id`](http://req.params.id/)`);
res.json(user);
} catch (err) {
next(err); // handled by errorHandler.middleware
}
};
```

