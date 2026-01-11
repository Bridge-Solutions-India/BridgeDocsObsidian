---

---
📌 **Helpers are reusable functions that assist a specific feature, module, or business flow.**

Helpers are reusable, feature-specific functions that simplify controller and service logic by handling common application-level tasks.

They are **application-aware utilities** that:

- Support controllers or services
- Encapsulate repetitive logic
- Are more **domain-specific** than utils

**Structure**

`helpers/
└── App.helper.js`

### Purpose

- Common response formatting
- Common checks
- App-level operations

app.helper.js - Common response formatting

`exports.successResponse = (res, data, message = "Success") => {
return res.status(200).json({
success: true,
message,
data,
});
};`

`exports.errorResponse = (res, message, status = 500) => {
return res.status(status).json({
success: false,
message,
});
};`

How to use it (controller)

`const { successResponse, errorResponse } = require("../helpers/App.helper");`

`exports.getProfile = async (req, res) => {
try {
const user = req.user;
successResponse(res, user, "Profile fetched");
} catch (err) {
errorResponse(res, "Failed to fetch profile");
}
};`

### What Should Go in Helpers?

✅ Response formatters

✅ Pagination logic

✅ Token helpers (app-specific)

✅ Permission checks

✅ Feature-based calculations

### What Should NOT Go in Helpers?

❌ Database connection

❌ Raw SQL queries

❌ Express routing

❌ Generic math/string utils

❌ Environment loading

### Best Practices

✅ Keep helpers small

✅ Group by feature

✅ Avoid side effects

✅ Name clearly

✅ Reusable but scoped


