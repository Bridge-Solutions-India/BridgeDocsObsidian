---

---
📌 **A model represents the structure of your data and how it is stored, validated, and accessed in the database.**

In simple terms:

> Models define “what data looks like” and provide methods to interact with that data.

A model is a schema-based representation of a database entity that defines fields, data types, validations, and database interaction methods.

### Purpose

- Define user schema
- Enforce validation
- Provide DB methods

**Role of Models in Your Architecture**

`Request
↓
Route
↓
Controller
↓
Model
↓
Database`

**Structure**

`models/
└── User.model.js`

Databases:

[[Mysql]]