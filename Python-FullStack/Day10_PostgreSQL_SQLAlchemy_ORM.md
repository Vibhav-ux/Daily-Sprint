# 🐍 Python Full Stack — Day 10: PostgreSQL + SQLAlchemy ORM
### Phase 2: Backend | Database Setup · Models · CRUD · Relationships · Alembic Migrations

---

> 📌 **Setup first:**
> ```
> pip install sqlalchemy psycopg2-binary alembic
> ```
> Or for async: `pip install asyncpg sqlalchemy[asyncio]`

---

## PART 1 — Why a Database? SQL vs ORM

**Raw SQL** works, but it's tedious: you write strings, parse results manually, and risk SQL injection.

**SQLAlchemy ORM** lets you define tables as Python classes and write queries in Python syntax. It translates to SQL automatically.

```
Python Class (Model)     →     SQLAlchemy ORM     →     SQL Table
User(name="Vibhav")      →     session.add(user)  →     INSERT INTO users ...
```

---

## PART 2 — Project Structure

```
project/
├── database.py      # Database engine + session factory
├── models.py        # ORM table models
├── schemas.py       # Pydantic models (request/response)
├── crud.py          # Database operations
└── main.py          # FastAPI routes
```

---

## PART 3 — `database.py`: Engine & Session

```python
# database.py
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, DeclarativeBase

# Replace with your actual credentials
DATABASE_URL = "postgresql://username:password@localhost:5432/daily_sprint_db"

engine = create_engine(DATABASE_URL, echo=True)  # echo=True logs SQL queries

SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

class Base(DeclarativeBase):
    pass

# Dependency for FastAPI routes
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

---

## PART 4 — `models.py`: ORM Table Definitions

```python
# models.py
from sqlalchemy import Column, Integer, String, Float, Boolean, DateTime, ForeignKey
from sqlalchemy.orm import relationship
from sqlalchemy.sql import func
from database import Base

class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True, index=True)
    name = Column(String(100), nullable=False)
    email = Column(String(200), unique=True, index=True, nullable=False)
    hashed_password = Column(String, nullable=False)
    is_active = Column(Boolean, default=True)
    created_at = Column(DateTime(timezone=True), server_default=func.now())

    # Relationship: one User → many Posts
    posts = relationship("Post", back_populates="author")

class Post(Base):
    __tablename__ = "posts"

    id = Column(Integer, primary_key=True, index=True)
    title = Column(String(200), nullable=False)
    content = Column(String, nullable=False)
    author_id = Column(Integer, ForeignKey("users.id"), nullable=False)

    author = relationship("User", back_populates="posts")
```

---

## PART 5 — `crud.py`: Database Operations

```python
# crud.py
from sqlalchemy.orm import Session
from models import User, Post

# ── CREATE ──
def create_user(db: Session, name: str, email: str, hashed_password: str) -> User:
    user = User(name=name, email=email, hashed_password=hashed_password)
    db.add(user)
    db.commit()
    db.refresh(user)    # loads auto-generated id and created_at
    return user

# ── READ ──
def get_user(db: Session, user_id: int) -> User | None:
    return db.get(User, user_id)

def get_user_by_email(db: Session, email: str) -> User | None:
    return db.query(User).filter(User.email == email).first()

def get_users(db: Session, skip: int = 0, limit: int = 10) -> list[User]:
    return db.query(User).offset(skip).limit(limit).all()

# ── UPDATE ──
def update_user_name(db: Session, user_id: int, new_name: str) -> User | None:
    user = get_user(db, user_id)
    if not user:
        return None
    user.name = new_name
    db.commit()
    db.refresh(user)
    return user

# ── DELETE ──
def delete_user(db: Session, user_id: int) -> bool:
    user = get_user(db, user_id)
    if not user:
        return False
    db.delete(user)
    db.commit()
    return True
```

---

## PART 6 — Wiring into FastAPI (`main.py`)

```python
# main.py (key snippet)
from fastapi import FastAPI, Depends, HTTPException
from sqlalchemy.orm import Session
from database import get_db, engine
import models, crud

models.Base.metadata.create_all(bind=engine)  # Create tables

app = FastAPI()

@app.post("/users/", status_code=201)
def register(name: str, email: str, db: Session = Depends(get_db)):
    existing = crud.get_user_by_email(db, email)
    if existing:
        raise HTTPException(status_code=400, detail="Email already registered")
    return crud.create_user(db, name, email, hashed_password="hashed_placeholder")

@app.get("/users/{user_id}")
def read_user(user_id: int, db: Session = Depends(get_db)):
    user = crud.get_user(db, user_id)
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return user
```

---

## 💡 Day 10 Challenge

Extend the model:
1. Add a `role` column to `User` (String, default `"student"`).
2. Write a `get_users_by_role(db, role)` function.
3. Add a `GET /users/role/{role}` route to the FastAPI app.
