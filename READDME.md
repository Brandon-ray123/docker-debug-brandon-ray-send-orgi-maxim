[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/9BX09cGc)


# 🐳 Todo App – Docker Debugging Lab

## 📌 Overview

This project is a simple **Todo application** built with:

* Frontend → Flask
* Backend → FastAPI
* Database → PostgreSQL
* Docker → to run everything together

The goal of this lab was to **find errors and fix them step by step** until the app works.

---

## 🏗️ How the App Works

There are 3 parts:

1. **Frontend**

   * Shows the web page
   * Sends data to backend

2. **Backend**

   * Handles requests (API)
   * Talks to the database

3. **Database**

   * Stores the todos

---

## 🔗 How Services Communicate

Inside Docker:

* Frontend → Backend

  ```
  http://backend:8000
  ```

* Backend → Database

  ```
  database:5432
  ```

👉 Important: we use **service names** (`backend`, `database`), not localhost.

---

## 🚀 How to Run

Run this command:

```bash
docker compose up --build
```

Open in browser:

* Frontend:

```
http://localhost:5050
```

* Backend:

```
http://localhost:8000
```

---

## 🛠️ Errors I Fixed

---

### ❌ 1. Dockerfile was incomplete

**Problem:**
It was missing the base image.

**Fix:**

```Dockerfile
FROM python:3.13-slim
```

---

### ❌ 2. Missing Python package

**Problem:**
Error:

```
python-multipart is not installed
```

**Fix:**
Added to backend `requirements.txt`:

```txt
python-multipart
```

---

### ❌ 3. Wrong backend URL

**Problem:**
Frontend was using:

```
http://api:8000
```

**Fix:**
Changed to:

```
http://backend:8000
```

👉 Because Docker uses service names.

---

### ❌ 4. Wrong ports

**Problem:**
Ports did not match the app.

**Fix:**

Backend:

```yaml
8000:8000
```

Frontend:

```yaml
5050:80
```

---

### ❌ 5. Port already in use

**Problem:**

```
address already in use
```

**Fix:**
Changed port from 5000 → 5050

---

### ⚠️ 6. Database connection error (temporary)

**Problem:**

```
connection refused
```

**Reason:**
Database was not ready yet.

**Solution:**
No change needed — it fixed itself when database started.

---

## ✅ Final Result

Now everything works:

* Frontend loads in browser ✅
* Backend works ✅
* Database works ✅

Example backend output:

```json
{"message": "Welcome to the Todo App!"}
```

---

## 🧠 What I Learned

* Docker services use **names to communicate**
* Ports must match the app
* Only the **left port (host)** can be changed
* Errors can happen during build or run
* Logs help find problems:

```bash
docker compose logs
```

---

## 🏁 Conclusion

I fixed all errors and successfully ran a **multi-container Docker app** with frontend, backend, and database.

---

## 👤 Author

Brandon-Ray
CTAI Student – Howest
