# Multi-Tenant SaaS Platform

A production-ready **multi-tenant SaaS application** built with **Node.js, Express, PostgreSQL, React, and Docker**.

This platform demonstrates how real-world SaaS systems are designed — with **tenant isolation**, **role-based access control (RBAC)**, **subscription limits**, and a **fully containerized setup** using Docker.

---

## Table of Contents

- Overview
- Features
- Roles & Access Control
- Tech Stack
- Architecture Overview
- Project Structure
- Environment Variables
- Running the Project (Docker)
- Database Migrations & Seeds
- UI Flow
- Error Handling
- Design Decisions
- Future Improvements

---

## Overview

This application follows a **multi-tenant SaaS architecture**, where multiple organizations (tenants) use the same system while keeping their data completely isolated.

The system supports three distinct roles:

- **Super Admin** – manages tenants at the system level
- **Tenant Admin** – manages users and projects within a tenant
- **User** – has read-only access to projects and tasks

Every request is secured using **JWT authentication**, enforced with **RBAC**, and filtered by **tenant scope** at the API and database level.

---

## Features

- JWT-based authentication
- Strict role-based access control (RBAC)
- Complete tenant data isolation
- Subscription plan limits (users & projects)
- Project and task management
- Read-only subscription plans
- Protected frontend routes
- Centralized error handling with toast notifications
- Fully Dockerized backend, frontend, and database
- One-command startup using Docker Compose

---

## Roles & Access Control

### Super Admin
- Login to the system
- View all registered tenants
- View tenant subscription plans and limits (read-only)
- **No access** to tenant-level data such as users, projects, or tasks

### Tenant Admin
- Login to tenant workspace
- View dashboard (users & projects count)
- Create and manage projects
- Create and manage tasks within projects
- Create and manage users (within subscription limits)

### User
- Login to tenant workspace
- View dashboard (projects count)
- View all projects within the tenant
- View project details and tasks
- Read-only access (no create/update permissions)

---

## Tech Stack

### Backend
- Node.js
- Express.js
- PostgreSQL
- JWT Authentication
- Docker

### Frontend
- React (Vite)
- React Router
- Axios
- Tailwind CSS
- React Toastify

### DevOps
- Docker
- Docker Compose

---

## Architecture Overview

```

Client (React)
|
v
JWT Authentication
|
v
API Server (Express)
|
v
RBAC & Tenant Enforcement
|
v
PostgreSQL (Tenant-Isolated Data)

```

### Key Concepts
- JWT middleware extracts user identity and role
- RBAC middleware enforces permissions per route
- Tenant middleware guarantees tenant data isolation
- Frontend protected routes prevent unauthorized navigation

---

## Project Structure

### Backend

```

backend/
├── src/
│   ├── app.js
│   ├── server.js
│   ├── config/
│   │   └── db.js
│   ├── controllers/
│   ├── routes/
│   ├── middleware/
│   ├── migrations/
│   ├── seeds/
│   └── utils/
├── Dockerfile
└── package.json

```

### Frontend

```

frontend/
├── src/
│   ├── api/
│   ├── auth/
│   ├── components/
│   ├── pages/
│   ├── utils/
│   ├── App.jsx
│   └── main.jsx
├── Dockerfile
└── package.json

````

---

## Environment Variables

### Backend (`backend/.env`)

```env
PORT=5000

DB_HOST=database
DB_PORT=5432
DB_NAME=saas_db
DB_USER=postgres
DB_PASSWORD=postgres

JWT_SECRET=your_jwt_secret_key
JWT_EXPIRES_IN=24h

FRONTEND_URL=http://frontend:3000
````

### Frontend (`frontend/.env`)

```env
REACT_APP_API_URL=http://backend:5000/api
```

---

## Running the Project (Docker)

### Prerequisites

* Docker
* Docker Compose

### Build & Start All Services

```bash
docker-compose up -d --build
```

### Stop All Services

```bash
docker-compose down
```

### View Logs

```bash
docker-compose logs backend
docker-compose logs frontend
```

### Access the Application

* Frontend: [http://localhost:3000](http://localhost:3000)
* Backend Health Check: [http://localhost:5000/api/health](http://localhost:5000/api/health)

---

## Database Migrations & Seeds

* Database migrations run automatically on backend startup
* Seed data is loaded automatically
* Initial data includes:

  * Super Admin account
  * Sample tenant
  * Tenant Admin user

This allows you to start testing the application immediately after running Docker.

---

## UI Flow

### Super Admin

```
Login → Tenants Dashboard
```

### Tenant Admin

```
Login → Dashboard → Projects → Project Details (Tasks) → Users
```

### User

```
Login → Dashboard → Projects → Project Details (Tasks)
```

> Tasks are intentionally scoped **inside Project Details** and not treated as a standalone module.

---

## Error Handling

* Backend uses consistent error response formats
* Frontend displays errors using toast notifications
* Global Axios interceptor handles:

  * `401` – Session expired
  * `403` – Unauthorized access
* Subscription limit errors are clearly surfaced to users

---

## Design Decisions

* Tasks are not assigned to individual users (not required by spec)
* No billing or plan upgrade UI (plans are configuration-based)
* All tenant users can view tenant-wide projects
* Subscription plans are read-only
* RBAC is enforced per route
* Super Admin is fully isolated from tenant-level data

These choices keep the implementation **simple, compliant, and focused**.

---

## Future Improvements

* Task assignment to users
* Subscription upgrade workflow
* Audit logs and activity tracking
* Pagination and search
* Production deployment (AWS, CI/CD pipelines)

---

## Conclusion

This project presents a **clean, scalable, and real-world implementation** of a multi-tenant SaaS system.

The focus is on:

* Correctness
* Security
* Tenant isolation
* Maintainability
* Docker-first workflows

It is designed to be easy to understand, easy to run, and ready for further extension.

```