markdown# Cloud Task Manager API

A full-stack task and project management REST API built with **FastAPI** and **MySQL**, featuring JWT authentication, ownership-based authorization, and a clean relational data model. Built as a hands-on cloud engineering portfolio project — designed for deployment on **AWS EC2**.

## Overview

This API allows users to sign up, log in securely, create projects, and manage tasks within those projects. Every resource is scoped to its owner, meaning users can only see and modify their own data — a core principle of real-world API security.

## Features

- **JWT-based authentication** — secure signup/login with hashed passwords (bcrypt) and token-based sessions
- **Ownership-based authorization** — users can only access their own projects and tasks
- **RESTful API design** — clean, predictable endpoints following REST conventions
- **Relational data modeling** — Users → Projects → Tasks, with cascading deletes
- **Partial updates** — PATCH support for updating individual task fields without overwriting the rest
- **Auto-generated interactive API docs** via FastAPI's built-in Swagger UI

## Tech Stack

| Layer | Technology |
|---|---|
| Backend Framework | FastAPI (Python) |
| Database | MySQL |
| ORM | SQLAlchemy |
| Authentication | JWT (python-jose) + bcrypt password hashing |
| Validation | Pydantic |
| Server | Uvicorn |
| Deployment Target | AWS EC2 |

## Architecture
Client → FastAPI App → SQLAlchemy ORM → MySQL Database
↓
JWT Auth Layer

**Data model:**
User (1) ──── (many) Project (1) ──── (many) Task

## API Endpoints

### Authentication
| Method | Endpoint | Description |
|---|---|---|
| POST | `/auth/signup` | Create a new user account |
| POST | `/auth/login` | Log in and receive a JWT access token |
| GET | `/auth/me` | Get the currently authenticated user's profile |

### Projects
| Method | Endpoint | Description |
|---|---|---|
| POST | `/projects/` | Create a new project |
| GET | `/projects/` | List all projects owned by the current user |
| GET | `/projects/{project_id}` | Get a specific project |
| DELETE | `/projects/{project_id}` | Delete a project (and its tasks) |

### Tasks
| Method | Endpoint | Description |
|---|---|---|
| POST | `/projects/{project_id}/tasks/` | Create a task within a project |
| GET | `/projects/{project_id}/tasks/` | List all tasks in a project |
| PATCH | `/projects/{project_id}/tasks/{task_id}` | Update a task (partial update) |
| DELETE | `/projects/{project_id}/tasks/{task_id}` | Delete a task |

## Local Setup

### Prerequisites
- Python 3.11+
- MySQL Server

### Installation

```bash
# Clone the repository
git clone https://github.com/thabitseif10/cloud-task-manager.git
cd cloud-task-manager

# Create and activate a virtual environment
python -m venv venv
venv\Scripts\activate      # Windows
source venv/bin/activate   # macOS/Linux

# Install dependencies
pip install -r requirements.txt

# Set up environment variables
# Copy .env.example to .env and fill in your own values
```

### Database Setup

```sql
CREATE DATABASE taskmanager;
```

### Run the server

```bash
uvicorn app.main:app --reload
```

Visit `http://127.0.0.1:8000/docs` for the interactive API documentation.

## Security Notes

- Passwords are never stored in plain text — only bcrypt hashes
- JWT tokens expire after a configurable time window
- All project/task routes verify resource ownership before returning data
- Environment variables (database credentials, secret keys) are excluded from version control via `.gitignore`

## Roadmap

- [ ] Deploy to AWS EC2
- [ ] Add a frontend (React)
- [ ] Migrate to AWS RDS for production database
- [ ] Add CI/CD pipeline with GitHub Actions
- [ ] Add automated tests (pytest)

## Author

**Thabit Seif** — IT Cloud Engineering student, Asia Pacific University
[LinkedIn](www.linkedin.com/in/thabit-seif-) | [GitHub](https://github.com/thabitseif10)

