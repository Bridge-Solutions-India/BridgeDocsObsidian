---

---

Middleware functions are functions that execute during the request–response cycle and can modify the request or response, perform checks, or terminate the request.

📌 **Middleware is a function that runs between the client request and the final response.**

It has access to:

- `req` (request)
- `res` (response)
- `next()` (to pass control)

**Request row**

`Client Request
↓
Middleware 1 (logging)
↓
Middleware 2 (auth)
↓
Middleware 3 (validation)
↓
Controller
↓
Response`

# Why Middleware Is Important

Middleware is used to:

- Authenticate users
- Authorize roles
- Validate input
- Log requests
- Handle errors
- Modify requests/responses

Without middleware:

- Repeated logic
- Messy controllers
- Poor security

**Structure**

`middleware/
├── App.middleware.js
├── ErrorHandler.middleware.js
├── init.js`

**app.middleware.js - application middleware**

`const jwt = require("jsonwebtoken");
const { appKeys } = require("../config/init");`

`exports.verifyToken = (req, res, next) => {
const token = req.headers.authorization;`

`if (!token) {
return res.status(401).json({ message: "No token provided" });
}`

`try {
const decoded = jwt.verify(token, appKeys.jwtSecret);
req.user = decoded; // attach user to request
next();
} catch (err) {
return res.status(401).json({ message: "Invalid token" });
}
};`

How to use it

`router.get(
"/profile",
authMiddleware.verifyToken,
UserController.profile
);`

**errorHandler.middleware.js — Global Error Handler**

- handles all application error in one place
- It is **extremely important** for building **stable, secure, and production-ready applications**.

`const Logger = require("../utils/Logger.util");`

`module.exports = (err, req, res, next) => {
Logger.error(err.message, err);`

`res.status(err.status || 500).json({
success: false,
message: err.message || "Internal Server Error",
});
};`

How to use it

`//should be the last app.use() in server`

`app.use(require("./middleware/ErrorHandler.middleware"));`

init.js - Middleware Loader

`middleware/init.js` is a **centralized initializer** that:

- Imports **all middleware**
- Configures them **in the correct order**
- Exports a single function to attach them to the Express app

It avoids cluttering `server.js` with many `app.use()` calls.

`const express = require("express");`

`const cors = require("cors");`

`const AppMiddleware = require("./app.middleware");`

`const ErrorHandler = require("./errorHandler.middleware");`

`module.exports = (app) => {`

`app.use(express.json());`

`app.use(express.urlencoded({ extended: true }));`

`app.use(cors());`

`app.use(AppMiddleware);`

`app.use(ErrorHandler);`

`};`

How to use it

`const initMiddleware = require("./app/middleware/init");`

`initMiddleware(app);`