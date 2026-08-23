# Database Connection — Beginner Guide

Yeh guide sirf ek cheez sikhati hai:

> **Node.js app ko PostgreSQL database se kaise connect karte hain?**

Mac aur Windows dono ke liye steps same hain. Jahan farq ho, wahan alag likha hai.

---

## Pehle simple samjho

Socho:

- **Database** = data ka box (users, products, etc.)
- **PostgreSQL** = woh software jo box manage karta hai
- **Sequelize** = Node.js aur database ke beech bridge
- **`.env`** = secret values (password, db name) — code mein hardcode nahi

Flow:

```
server.js  →  config/db.js  →  PostgreSQL
```

---

## Step 1 — Packages install karo

Project folder (`my-backend`) mein:

```bash
npm install express cors dotenv sequelize pg pg-hstore
npm install -D nodemon
```

Hum connection ke liye mainly ye use karte hain:
- `dotenv` → `.env` padhna
- `sequelize` → connection banana
- `pg` → PostgreSQL driver

**Warning:** `npm audit fix --force` mat chalana.  
Agar Sequelize toot jaye to:

```bash
npm install sequelize@6.37.8
```

---

## Step 2 — `.env` file banao

Project root mein `.env` banao:

```env
PORT=3000
DB_HOST=localhost
DB_PORT=5432
DB_NAME=backend_db
DB_USER=postgres
DB_PASSWORD=123456
```

| Value | Matlab |
|-------|--------|
| `DB_HOST` | Database kahan chal rahi hai (local = localhost) |
| `DB_PORT` | Postgres ka port (default 5432) |
| `DB_NAME` | Database ka naam |
| `DB_USER` | Login username |
| `DB_PASSWORD` | Login password |

Apna real Postgres password yahan likho.

---

## Step 3 — Database banao (PostgreSQL)

Pehle PostgreSQL install + running hona chahiye.

### Mac
Terminal mein:

```bash
psql -U postgres
```

### Windows
Start Menu se **SQL Shell (psql)** kholo, ya:

```bash
psql -U postgres
```

Password do, phir ye likho:

```sql
CREATE DATABASE backend_db;
\q
```

**Yaad rakho:**  
`CREATE DATABASE` seedha Terminal/CMD mein nahi chalta.  
Pehle `psql` ke andar jao, phir SQL likho.

---

## Step 4 — Connection file likho (`config/db.js`)

Folder banao:

```bash
mkdir config
```

File: `config/db.js`

```js
require("dotenv").config();
const { Sequelize } = require("sequelize");

const sequelize = new Sequelize(
  process.env.DB_NAME,
  process.env.DB_USER,
  process.env.DB_PASSWORD,
  {
    host: process.env.DB_HOST,
    port: process.env.DB_PORT,
    dialect: "postgres",
    logging: false,
  }
);

module.exports = sequelize;
```

### Line by line (beginner)

1. `dotenv` → `.env` se values uthao  
2. `Sequelize` → connection tool lao  
3. `new Sequelize(...)` → database se link banao  
4. `module.exports` → ye link dusri files ko do  

---

## Step 5 — Test file likho (`server.js`)

```js
require("dotenv").config();
const sequelize = require("./config/db");

async function startServer() {
  try {
    await sequelize.authenticate();
    console.log("Database connected successfully");
  } catch (error) {
    console.error("Database connection failed:", error.message);
  }
}

startServer();
```

`authenticate()` ka matlab:  
**“Database, kya tum sun rahe ho?”**

---

## Step 6 — Script set karo (`package.json`)

```json
"scripts": {
  "start": "node server.js",
  "dev": "nodemon server.js"
}
```

File ka naam `server.js` hai, isliye script bhi `server.js` honi chahiye — `index.js` nahi.

---

## Step 7 — Run karo

```bash
npm run dev
```

### Success
```text
Database connected successfully
```
Matlab: connection theek hai.

### Fail
```text
Database connection failed: ...
```

Check karo:
1. PostgreSQL chal rahi hai?
2. Database `backend_db` bani hai?
3. `.env` ka user/password sahi hai?

---

## Short summary

| Step | File / Kaam |
|------|-------------|
| 1 | Packages install |
| 2 | `.env` values |
| 3 | Postgres mein database banao |
| 4 | `config/db.js` — connection |
| 5 | `server.js` — test |
| 6 | `npm run dev` |

Bas itna: **`.env` → `db.js` → `authenticate()` → connected**
