---

---

📌 `**server.js**`** is the main entry point of your Node.js / Express application.**

	server.js is the bootstrap file of an Express application that initializes the server, configures middleware, routes, and database connections, and starts listening for client requests.


It is responsible for:
1. Creating and configuring the Express app
2. Registering middleware
3. Registering routes
4. Connecting to databases
5. Starting the HTTP server

==`Structure`==

server.js
├─ Load environment variables
├─ Initialize app
├─ Initialize config & middleware
├─ Initialize database connections
├─ Register routes
├─ Register error handler
├─ Start server`

==`Code`==
```
const express = require("express");
const path = require("path");

require("dotenv").config();

const app = express();
const initConfig = require("./app/config/init");

initConfig();

const initMiddleware = require("./app/middleware/init");
initMiddleware(app);

require("./app/database/init");

app.use("/", require("./app/routes/App.routes"));
app.use("/auth", require("./app/routes/Auth.routes"));
app.use("/dashboard", require("./app/routes/Dashboard.routes"));

const errorHandler = require("./app/middleware/ErrorHandler.middleware");

app.use(errorHandler);

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
console.log(`Server running on http://localhost:${PORT}`);
});
```
