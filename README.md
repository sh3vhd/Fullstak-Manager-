<div align="center">

<img src="https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=white" />
<img src="https://img.shields.io/badge/TypeScript-5-3178C6?style=for-the-badge&logo=typescript&logoColor=white" />
<img src="https://img.shields.io/badge/Node.js-20-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" />
<img src="https://img.shields.io/badge/MySQL-8-4479A1?style=for-the-badge&logo=mysql&logoColor=white" />
<img src="https://img.shields.io/badge/Docker-ready-2496ED?style=for-the-badge&logo=docker&logoColor=white" />

# ✅ TaskFlow — Fullstack Task Manager

**A production-ready, clean-architecture task management app**  
*Built to demonstrate real Fullstack engineering skills*

[🚀 Quick Start](#-quick-start) • [📐 Architecture](#-architecture) • [🔌 API Reference](#-api-reference) • [🐳 Docker](#-docker-deployment) • [Русский](./README.ru.md)

</div>

---

## 🌟 Features

| Category | Details |
|----------|---------|
| **Auth** | JWT-based login & registration, bcrypt password hashing, token refresh |
| **Tasks** | Full CRUD — create, edit, delete, bulk status updates |
| **Status** | Three states: `Todo` → `In Progress` → `Done` |
| **Priority** | Low / Medium / High with visual indicators |
| **Filtering** | Filter by status, priority, search by title/description |
| **Pagination** | Server-side pagination with metadata |
| **Validation** | Client-side + server-side, field-level errors |
| **Security** | Helmet, CORS, Rate limiting, SQL injection prevention |
| **Docker** | Full Docker Compose setup for one-command launch |

---

## 🛠 Tech Stack

### Frontend
- **React 18** + **TypeScript** — type-safe component architecture
- **React Router v6** — client-side routing with protected routes
- **Zustand** — lightweight global state (auth)
- **TanStack Query v5** — server state, caching, background refetching
- **Axios** — HTTP client with request/response interceptors
- **Tailwind CSS** — utility-first responsive styling
- **react-hot-toast** — toast notifications

### Backend
- **Node.js** + **Express.js** — REST API server
- **MySQL 8** + **mysql2** — relational database with connection pooling
- **JWT** (jsonwebtoken) — stateless authentication
- **bcryptjs** — secure password hashing (cost factor 12)
- **express-validator** — declarative input validation
- **Helmet** — security HTTP headers
- **express-rate-limit** — brute force protection

---

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ and npm
- MySQL 8.0 running locally
- (Optional) Docker + Docker Compose

### Option A — Manual Setup

**1. Clone the repository**
```bash
git clone https://github.com/your-username/fullstack-task-manager.git
cd fullstack-task-manager
```

**2. Set up the backend**
```bash
cd backend
cp .env.example .env       # Copy env template
# Edit .env with your MySQL credentials
npm install
npm run migrate            # Creates tables
npm run dev                # Starts on :5000
```

**3. Set up the frontend**
```bash
cd frontend
npm install
npm run dev                # Starts on :3000
```

**4. Open in browser**
```
http://localhost:3000
```

### Option B — Docker (Recommended)

```bash
git clone https://github.com/your-username/fullstack-task-manager.git
cd fullstack-task-manager

# Copy and configure environment
cp .env.example .env

# Start everything with one command
docker compose up --build

# App available at:
# Frontend  →  http://localhost:3000
# API       →  http://localhost:5000
# DB        →  localhost:3306
```

---

## 📐 Architecture

```
fullstack-task-manager/
│
├── 🖥️  frontend/                    # React + TypeScript SPA
│   └── src/
│       ├── components/
│       │   ├── auth/                # Login, Register pages
│       │   ├── tasks/               # Dashboard, TaskList, TaskForm
│       │   ├── layout/              # AppLayout, ProtectedRoute, Sidebar
│       │   └── ui/                  # Reusable: Button, Input, Modal, Badge
│       ├── hooks/                   # useAuth, useTasks (React Query)
│       ├── services/                # api.ts — Axios client + all API calls
│       ├── store/                   # authStore.ts (Zustand)
│       └── types/                   # Shared TypeScript interfaces
│
└── ⚙️  backend/                     # Node.js + Express API
    └── src/
        ├── config/                  # database.js, migrate.js
        ├── models/                  # UserModel.js, TaskModel.js (SQL layer)
        ├── controllers/             # authController, taskController (HTTP layer)
        ├── middleware/              # auth.js (JWT), validators.js, errorHandler.js
        ├── routes/                  # authRoutes.js, taskRoutes.js
        └── server.js                # Express app entry point
```

### Design Patterns
- **Repository Pattern** — Models encapsulate all SQL, controllers never write queries
- **Middleware Chain** — validate → authenticate → controller
- **Service Layer** — Frontend's `api.ts` centralizes all HTTP calls
- **Custom Hooks** — `useTasks` / `useAuth` decouple data fetching from UI

---

## 🔌 API Reference

Base URL: `http://localhost:5000/api`

### Auth Endpoints

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| `POST` | `/auth/register` | ❌ | Create new account |
| `POST` | `/auth/login` | ❌ | Login, get JWT |
| `GET` | `/auth/me` | ✅ | Get current user |

### Task Endpoints

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| `GET` | `/tasks` | ✅ | List tasks (paginated, filterable) |
| `POST` | `/tasks` | ✅ | Create task |
| `GET` | `/tasks/stats` | ✅ | Task counts by status |
| `GET` | `/tasks/:id` | ✅ | Get single task |
| `PATCH` | `/tasks/:id` | ✅ | Update task (partial) |
| `DELETE` | `/tasks/:id` | ✅ | Delete task |

#### Query Parameters for `GET /tasks`
```
page      integer   Page number (default: 1)
limit     integer   Items per page (default: 10, max: 100)
status    string    Filter: todo | in_progress | done
priority  string    Filter: low | medium | high
search    string    Search in title and description
sort      string    Sort by: created_at | due_date | priority | title
order     string    ASC | DESC (default: DESC)
```

#### Example Requests

**Register:**
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice","email":"alice@example.com","password":"Secret123"}'
```

**Create Task:**
```bash
curl -X POST http://localhost:5000/api/tasks \
  -H "Authorization: Bearer <your_jwt_token>" \
  -H "Content-Type: application/json" \
  -d '{"title":"Build awesome app","priority":"high","due_date":"2026-03-01"}'
```

**Response Format:**
```json
{
  "success": true,
  "data": {
    "tasks": [...],
    "pagination": {
      "total": 42,
      "pages": 5,
      "page": 1,
      "limit": 10,
      "hasNextPage": true,
      "hasPrevPage": false
    }
  }
}
```

---

## 🐳 Docker Deployment

### Development
```bash
docker compose up
```

### Production Build
```bash
# Frontend builds to optimized static files served by Nginx
# Backend runs in production mode (no nodemon)
docker compose -f docker-compose.yml up -d --build
```

### Useful Commands
```bash
docker compose logs -f backend    # Follow API logs
docker compose exec db mysql -u root -p task_manager  # MySQL CLI
docker compose down -v            # Stop and remove volumes
```

---

## 🗄️ Database Schema

```sql
-- Users table
users (
  id            INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  name          VARCHAR(100) NOT NULL,
  email         VARCHAR(255) NOT NULL UNIQUE,
  password_hash VARCHAR(255) NOT NULL,
  created_at    TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at    TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
)

-- Tasks table  
tasks (
  id          INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  user_id     INT UNSIGNED NOT NULL,           -- FK → users.id
  title       VARCHAR(255) NOT NULL,
  description TEXT,
  status      ENUM('todo','in_progress','done') DEFAULT 'todo',
  priority    ENUM('low','medium','high')       DEFAULT 'medium',
  due_date    DATE,
  position    INT UNSIGNED DEFAULT 0,           -- for custom ordering
  created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
)
```

---

## 🔒 Security Highlights

- **Passwords** hashed with bcrypt (cost 12) — never stored in plaintext
- **JWT** tokens expire in 7 days, validated on every request
- **User isolation** — every query is scoped to `user_id`, users can't access others' data
- **Rate limiting** — auth endpoints limited to 10 req/15min
- **Input validation** — all inputs sanitized and validated before touching the DB
- **Helmet** — sets 11 security-related HTTP headers automatically
- **SQL injection** prevented by parameterized queries (`?` placeholders)

---

## 📝 License

MIT — free for personal and commercial use.

---

<div align="center">
  <sub>Built with ❤️ as a clean architecture showcase project</sub>
</div>
