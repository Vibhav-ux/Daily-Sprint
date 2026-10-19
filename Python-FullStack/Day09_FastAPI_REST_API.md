# 🐍 Python Full Stack — Day 09: FastAPI — Building REST APIs
### Phase 0 → Phase 2 Bridge | Routes · Pydantic Models · CRUD · Swagger UI

---

> 📌 **Setup first (run once):**
> ```
> pip install fastapi uvicorn[standard]
> ```
> Start dev server: `uvicorn main:app --reload`
> Open Swagger UI: http://127.0.0.1:8000/docs

---

## PART 1 — Why FastAPI?

FastAPI is the modern Python web framework that powers production APIs. It gives you:
- ✅ **Automatic Swagger/OpenAPI docs** — free documentation UI
- ✅ **Pydantic validation** — invalid requests are rejected automatically
- ✅ **asyncio native** — high performance, non-blocking
- ✅ **Type hints everywhere** — IDE autocomplete + runtime validation
- ✅ **Used by:** Netflix, Microsoft, Uber, Spotify

---

## PART 2 — Your First FastAPI Application

```python
# main.py
from fastapi import FastAPI

app = FastAPI(title="Daily Sprint API", version="1.0.0")

# ── GET Route ──
@app.get("/")
async def root():
    return {"message": "Welcome to Daily Sprint API 🚀"}

@app.get("/health")
async def health_check():
    return {"status": "healthy", "service": "Daily Sprint"}
```

---

## PART 3 — Path Parameters & Query Parameters

```python
from fastapi import FastAPI, HTTPException

app = FastAPI()

fake_db = {
    1: {"name": "Vibhav Shukla", "role": "Student"},
    2: {"name": "Alice", "role": "Engineer"},
}

# ── Path Parameter: /users/1 ──
@app.get("/users/{user_id}")
async def get_user(user_id: int):
    if user_id not in fake_db:
        raise HTTPException(status_code=404, detail=f"User {user_id} not found")
    return fake_db[user_id]

# ── Query Parameters: /users?limit=5&skip=0 ──
@app.get("/users")
async def list_users(limit: int = 10, skip: int = 0):
    users = list(fake_db.values())
    return {"users": users[skip : skip + limit], "total": len(users)}
```

---

## PART 4 — Pydantic Models: Request & Response Validation

Pydantic validates incoming JSON automatically. If a field is missing or has wrong type, it returns a clear `422 Unprocessable Entity` error.

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel, EmailStr, Field
from typing import Optional

app = FastAPI()

# ── Pydantic Models ──
class UserCreate(BaseModel):
    name: str = Field(..., min_length=2, max_length=50)
    email: str
    age: int = Field(..., ge=0, le=150)
    role: Optional[str] = "student"

class UserResponse(BaseModel):
    id: int
    name: str
    email: str
    role: str

# ── In-memory database (for learning) ──
users_db: dict[int, dict] = {}
next_id = 1

# ── POST: Create a User ──
@app.post("/users", response_model=UserResponse, status_code=201)
async def create_user(user: UserCreate):
    global next_id
    new_user = {"id": next_id, **user.model_dump()}
    users_db[next_id] = new_user
    next_id += 1
    return new_user

# ── GET: Read a User ──
@app.get("/users/{user_id}", response_model=UserResponse)
async def get_user(user_id: int):
    if user_id not in users_db:
        raise HTTPException(status_code=404, detail="User not found")
    return users_db[user_id]

# ── PUT: Update a User ──
@app.put("/users/{user_id}", response_model=UserResponse)
async def update_user(user_id: int, updates: UserCreate):
    if user_id not in users_db:
        raise HTTPException(status_code=404, detail="User not found")
    users_db[user_id].update(updates.model_dump())
    return users_db[user_id]

# ── DELETE: Remove a User ──
@app.delete("/users/{user_id}", status_code=204)
async def delete_user(user_id: int):
    if user_id not in users_db:
        raise HTTPException(status_code=404, detail="User not found")
    del users_db[user_id]
```

---

## PART 5 — API HTTP Status Codes Cheat Sheet

| Code | Meaning | When to use |
|------|---------|-------------|
| `200 OK` | Success | GET requests |
| `201 Created` | Resource created | POST requests |
| `204 No Content` | Success, no body | DELETE requests |
| `400 Bad Request` | Client error (bad input) | Validation failed |
| `401 Unauthorized` | Not authenticated | No token provided |
| `403 Forbidden` | Authenticated but no access | Wrong role |
| `404 Not Found` | Resource doesn't exist | User/item missing |
| `422 Unprocessable Entity` | Validation error | Pydantic schema mismatch |
| `500 Internal Server Error` | Server crashed | Unhandled exception |

---

## 💡 Day 09 Challenge

Add two more routes to the application:
1. `GET /users` — returns all users as a list.
2. `GET /users/search?name=Vibhav` — search users by partial name match.
