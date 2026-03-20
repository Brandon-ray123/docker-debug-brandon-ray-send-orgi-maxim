[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/9BX09cGc)

# 🐳 Todo App – Full Debugging Guide (Step by Step)

## 📌 1. What is this project?

This project is a Todo App made with:

* Frontend → Flask (UI)
* Backend → FastAPI (API)
* Database → PostgreSQL
* Docker → to run everything together

We used Docker Compose to run all services at the same time.

---

## 📌 2. What is the goal?

The project was **broken at the beginning**.

My task was:

* Run the project
* See errors
* Understand each error
* Fix them one by one

---

## 📌 3. How I started

I ran:

```bash
docker compose up
```

👉 At first, it **did NOT work**

---

# 🛠️ 4. Errors I found and how I fixed them

---

## ❌ ERROR 1 — Dockerfile incomplete

### Problem

Frontend Dockerfile only had:

```Dockerfile
CMD ["python", "app.py"]
```

👉 Missing `FROM`

Docker needs a base image.

---

### Fix

I added:

```Dockerfile
FROM python:3.13-slim
```

---

## ❌ ERROR 2 — pip install failed

### Problem

During build:

```bash
pip install -r requirements.txt FAILED
```

👉 Something wrong inside `requirements.txt`

---

### Fix

Checked dependencies and corrected them.

---

## ❌ ERROR 3 — Missing python-multipart

### Problem

Backend error:

```bash
Form data requires "python-multipart"
```

👉 FastAPI needs this for forms.

---

### Fix

I added this to **backend requirements.txt**:

```txt
python-multipart
```

---

### Important understanding

❗ I did NOT install it manually in terminal
❗ I added it inside requirements.txt

Because Docker installs dependencies from there.

---

## ❌ ERROR 4 — Backend could not connect to database

### Problem

Error:

```bash
connection refused
```

---

### My confusion

I thought:
👉 something is wrong with PostgreSQL

---

### Real explanation

👉 Database was **not ready yet**
👉 Backend started faster than database

---

### Final understanding

✔ This is normal
✔ No fix needed
✔ When database starts → everything works

---

## ❌ ERROR 5 — Wrong backend URL in frontend

### Problem

Frontend used:

```text
http://api:8000
```

---

### Why this is wrong

There is NO service called `api`

---

### Fix

Changed to:

```text
http://backend:8000
```

---

### Important concept

Inside Docker:

👉 Services talk using **service names**

---

## ❌ ERROR 6 — Wrong port mapping

### Problem

Ports did not match actual app ports.

---

### Backend log showed:

```bash
Running on port 8000
```

---

### Fix

```yaml
backend:
  ports:
    - "8000:8000"
```

---

### Frontend log showed:

```bash
Running on port 80
```

---

### Fix

```yaml
frontend:
  ports:
    - "5000:80"
```

---

## ❌ ERROR 7 — Port already in use

### Problem

Error:

```bash
address already in use
```

---

### Cause

Port 5000 already used on my Mac.

---

### Fix

Changed to:

```yaml
frontend:
  ports:
    - "5050:80"
```

---

### Important concept

✔ Left side = my computer
✔ Right side = container

👉 Only left side can change

---

## ❌ ERROR 8 — Browser not opening

### Problem

I opened:

```text
http://localhost
```

👉 It did not work

---

### Fix

I used correct port:

```text
http://localhost:5050
```

---

## 📌 5. Final Working Setup

---

### Backend

```yaml
backend:
  build: ./backend
  ports:
    - "8000:8000"
```

---

### Frontend

```yaml
frontend:
  build: ./frontend
  ports:
    - "5050:80"
  environment:
    BACKEND_URL: http://backend:8000
```

---

### Database

```yaml
database:
  image: postgres:17-alpine
```

---

## 📌 6. Final Result

After fixing everything:

### Frontend works

👉 Shows Todo App page

### Backend works

👉 Shows:

```json
{"message": "Welcome to the Todo App!"}
```

### Database works

👉 Accepts connections

---

## 📌 7. What I learned

* Dockerfile must start with `FROM`
* requirements.txt is very important
* Services use names like `backend`, not localhost
* Ports must match the app
* Only left port can change
* Logs help find errors:

```bash
docker compose logs
```

---

## 📌 8. How to run again (for future)

If I forget:

```bash
docker compose down
docker compose up --build
```

Then open:

```text
http://localhost:5050
```

---

## 🏁 Conclusion

I successfully fixed all errors and made the app work.

Now I understand:

* Docker build
* Docker run
* Debugging errors
* Service communication
* Port mapping

---

## 👤 Author

Brandon-Ray
CTAI Student – Howest
