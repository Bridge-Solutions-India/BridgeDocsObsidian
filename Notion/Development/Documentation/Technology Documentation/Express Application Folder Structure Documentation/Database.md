---

---

**The **`**database/**`** folder is responsible for initializing, managing, and exporting live database connections.**


The `config` folder defines *what values to use*, while the `database` folder defines *how to connect and interact with databases*.

It contains **only infrastructure code**:

- Opening connections
- Handling retries / failures
- Exporting clients
- Managing multiple databases

❌ It does **not** contain:

- Business logic
- Request handling
- Environment variables
- Secrets

**Structure**

`database/
├── Mongo.database.js
├── Redis.database.js
├── init.js`

**mongo.database.js - MongoDB Connection**

- Maintain single shared connection

`const mongoose = require("mongoose");
const { dbKeys } = require("../config/init");
const Logger = require("../utils/Logger.util");`

`const connectMongo = async () => {
try {
await mongoose.connect(dbKeys.mongo.uri, {
autoIndex: false,
serverSelectionTimeoutMS: 5000,
});`

[`Logger.info`](http://logger.info/)`("MongoDB connected successfully");`

`} catch (error) {
Logger.error("MongoDB connection failed", error);
process.exit(1);
}
};`

`module.exports = connectMongo;`

How to use it

`const connectMongo = require("../database/Mongo.database");`

`await connectMongo();`

**redis.database.js - Redis client layer**

- creates Redis client
- handles connection events
- single Redis instance

`const Redis = require("ioredis");
const { dbKeys } = require("../config/init");`

`const redisClient = new Redis({
host: dbKeys.redis.host,
port: dbKeys.redis.port,
password: dbKeys.redis.password,
maxRetriesPerRequest: 3,
});`

`redisClient.on("connect", () => {
console.log("Redis connected");
});`

`redisClient.on("error", (err) => {
console.error("Redis error:", err);
});`

`module.exports = redisClient;`

How to use it

`const redis = require("../database/Redis.database");`

`await redis.set("user:1", JSON.stringify(user));`

**init.js - Database BootStrapper**

### Purpose

- Decide **which databases to initialize**
- Control startup order
- Prevent unnecessary connections

`const { dbConf } = require("../config/init");`

`const { connectMysql } = require("./mysql.database");`

`const initDatabase = async () => {`

`if (dbConf.mysql.enabled) {`

`await connectMysql();`

`}`

`};`

`module.exports = initDatabase;`

How to use it

`const initDatabase = require("./database/init");`

`await initDatabase();`

