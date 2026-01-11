---

---

The `utils` folder contains reusable helper modules that provide common functionality such as logging, formatting, encryption, or validation, independent of application flow.



These are **generic functions or classes** that:

- Do **one small job**
- Are **not tied to business logic**
- Do **not depend on Express request/response**
- Can be reused in controllers, middleware, database, etc.

# What Belongs in `utils/`

✅ Logging

✅ Date formatting

✅ Password hashing

✅ Token helpers

✅ File helpers

✅ Response formatters

❌ Controllers

❌ Routes

❌ Database connections

❌ Config loading

**Structure**

`utils/
└── Logger.util.js`

logger.util.js - Unified logging for the application

`const { appConf } = require("../config/init");`

`const log = (level, message, meta = null) => {
const timestamp = new Date().toISOString();
const output = {
timestamp,
level,
message,
...(meta && { meta }),
};`

`if (appConf.env === "production") {
// Send logs to external service
console.log(JSON.stringify(output));
} else {
console.log([${level}], message, meta || "");
}
};`

`module.exports = {
info: (msg, meta) => log("INFO", msg, meta),
warn: (msg, meta) => log("WARN", msg, meta),
error: (msg, meta) => log("ERROR", msg, meta),
};`