---

---

The `routes/` folder defines the HTTP endpoints of your application.

	Routes define how an application responds to client requests by mapping HTTP methods and paths to controller logic.

It acts as the **entry point for client requests** and is responsible for:
- Mapping URLs to controller functions
- Applying middle ware
- Organizing endpoints by feature/domain

❌ Routes should **not** contain:
- Business logic
- Database code
- Validation logic (heavy)
- Configuration loading

==`Structure`==

routes/
├── App.routes.js
├── Auth.routes.js
├── Dashboard.routes.js`

==`Request Flow`==

```
Client Request
↓
Route
↓
Middleware (Auth / Validation)
↓
Controller
↓
Service / Model
↓
Response`
```

==`auth.routes.js - Router for authentication routes`==
- routes can be both public and protected
- protected routes are passed through middleware for validation / authentication

```
const express = require("express");
const router = express.Router();

const AuthController = require("../controllers/User.controller");
const authMiddleware = require("../middleware/App.middleware");

/**

@route POST /auth/login

@desc Login user

@access Public
*/
router.post("/login", AuthController.login);

/**

@route POST /auth/register

@desc Register user

@access Public
*/
router.post("/register", AuthController.register);

/**

@route GET /auth/profile

@desc Get logged-in user profile

@access Private
*/
router.get(
"/profile",
authMiddleware.verifyToken,
AuthController.profile
);

module.exports = router;
```