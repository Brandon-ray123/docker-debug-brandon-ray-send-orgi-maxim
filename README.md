[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/9BX09cGc)

# 🐳 Todo App – Docker Debugging Lab

## 📌 Overview

This project is a **microservice-based Todo application** built using:

* **Frontend:** Flask
* **Backend:** FastAPI
* **Database:** PostgreSQL
* **Containerization:** Docker & Docker Compose

The goal of this lab was to **debug a broken Docker setup**, identify errors, and fix them step by step until the application runs correctly.

---

## 🏗️ Architecture

The application consists of 3 services:

1. **Frontend (Flask)**

   * Provides the user interface
   * Communicates with backend API

2. **Backend (FastAPI)**

   * Handles API requests
   * Connects to the database

3. **Database (PostgreSQL)**

   * Stores todo data

---

## 🔗 Service Communication

Inside Docker, services communicate using **service names**:

* Frontend → Backend:

  ```
  http://backend:8000
  ```

* Backend → Database:

  ```
  database:5432
  ```

---

## 🚀 How to Run the Project

### 1. Build and start containers

```bash
docker compose up --build
```

### 2. Open in browser

* Frontend:

```
http://localhost:5050
```

* Backend API:

```
http://localhost:8000
```

---

## 🛠️ Debugging Process

During this lab, multiple errors were encountered and fixed:

---

### ❌ 1. Missing Base Image in Dockerfile

**Problem:**
Dockerfile was incomplete (missing `FROM` instruction)

**Fix:**
Added:

```Dockerfile
FROM python:3.13-slim
```

---

### ❌ 2. Incorrect Python Dependencies

**Problem:**
Application failed due to missing dependency:

```
Form data requires "python-multipart"
```

**Fix:**
Added to backend `requirements.txt`:

```txt
python-multipart
```

---

### ❌ 3. Wrong Service Name in Frontend

**Problem:**
Frontend was using:

```
http://api:8000
```

**Issue:**
`api` is not a valid Docker service name.

**Fix:**
Updated to:

```
http://backend:8000
```

---

### ❌ 4. Incorrect Port Mapping

**Problem:**
Ports did not match actual application ports.

**Fix:**

* Backend:

```yaml
"8000:8000"
```

* Frontend:

```yaml
"5050:80"
```

---

### ❌ 5. Port Conflict Error

**Problem:**

```
bind: address already in use
```

**Cause:**
Port 5000 was already used on the host machine.

**Fix:**
Changed to another port:

```yaml
"5050:80"
```

---

### ⚠️ 6. Temporary Database Connection Error

**Problem:**

```
connection refused
```

**Cause:**
Backend tried to connect before database was fully ready.

**Resolution:**
No change required — database starts shortly after and system works correctly.

---

## ✅ Final Result

After fixing all issues:

* All containers run successfully ✅
* Frontend UI loads in browser ✅
* Backend API responds correctly ✅
* Database accepts connections ✅

Example backend response:

```json
{"message": "Welcome to the Todo App!"}
```

---

## 📷 Screenshots

* Todo App interface working
* Backend API response visible in browser

---

## 🧠 Key Learnings

* Docker containers communicate using **service names**
* Ports must match the application’s internal ports
* Only the **host port (left side)** can be changed freely
* Build errors ≠ runtime errors
* Logs are essential for debugging:

  ```bash
  docker compose logs
  ```

---

## 🏁 Conclusion

This lab demonstrates how to:

* Debug Docker builds and runtime errors
* Fix service communication issues
* Configure proper port mappings
* Successfully run a multi-container application

---

## 👤 Author

Brandon-Ray
CTAI Student – Howest
