---

---
The `config` folder centralizes application settings, environment variables, feature flags, and secrets, keeping them separate from business logic and infrastructure code.

Without `config/`:

- Credentials get hard-coded
- Environment switching is painful
- Code duplication increases
- Security risks increase

With `config/`:

- One source of truth
- Easy dev / test / prod separation
- Safer secrets management
- Cleaner architecture

### **Files in config:**

`config/
├── db.conf.js
├── app.conf.js
├── app.keys.js
├── db.keys.js
├── init.js`


**db.config.js - Database behavior configuration**

![[image.png]]


**db.keys.js - Database Secrets & Credentials**

- Stores sensitive database connection values
- acts as a bridge between .env and database code

![[image 1.png]]


**app.config.js - Application Behavior Settings**

- Defines how the application behaves globally
- Values
    - App name
    - Port
    - Environment
    - Logging level
    - API versioning

![[image 2.png]]

**app.keys.js - Application Secrets**

- Stores non-DB secrets used by the application
- All secrets required by the application

![[image 3.png]]

init.js - Configuration Boot strapper

- Loads environment variables
- Validates required config
- Exports final config objects
- If some environment variables are not available stops the application

![[image 4.png]]

How to use config/init.js

Code:

//validates the dependencies

require(””./app/config/init);