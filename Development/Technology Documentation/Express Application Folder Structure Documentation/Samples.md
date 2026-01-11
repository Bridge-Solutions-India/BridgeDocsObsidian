---

---

	📌 The samples/ folder contains example or template files** that help developers understand the configuration and environment setup of the project.

It usually includes:
- `.env.sample` → Environment variable template
- `db.conf.sample` → Database configuration template
- `app.conf.sample` → App configuration template
- `app.keys.sample` → Secret keys template

### Purpose of `samples/`
1. **Guidance for new developers**
    - Shows the required variables and their structure
    - Example: `.env.sample` shows `PORT=3000`, `DB_HOST=localhost`
2. **Version control friendly**
    - Actual `.env` or key files are not committed (contain secrets)
    - Only `.sample` files are committed
3. **Template for setup scripts / deployment**
    - Used to generate `.env` during automated deployment or CI/CD

### Why `samples/` Is Important
- **Onboarding:** New developers know required environment variables
- **Security:** Keeps real secrets out of version control
- **Consistency:** Standardizes configs across environments (dev/test/prod)
- **Documentation:** Serves as living documentation of configs
