<div align="center">

<img src="https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=white" />
<img src="https://img.shields.io/badge/TypeScript-5-3178C6?style=for-the-badge&logo=typescript&logoColor=white" />
<img src="https://img.shields.io/badge/Node.js-20-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" />
<img src="https://img.shields.io/badge/MySQL-8-4479A1?style=for-the-badge&logo=mysql&logoColor=white" />
<img src="https://img.shields.io/badge/Docker-ready-2496ED?style=for-the-badge&logo=docker&logoColor=white" />

# ✅ TaskFlow — Fullstack Менеджер Задач

**Готовое к продакшену приложение для управления задачами на чистой архитектуре**  
*Создано для демонстрации реальных Fullstack-навыков*

[🚀 Быстрый старт](#-быстрый-старт) • [📐 Архитектура](#-архитектура) • [🔌 API](#-api-справочник) • [🐳 Docker](#-docker-деплой) • [English](./README.md)

</div>

---

## 🌟 Возможности

| Категория | Детали |
|-----------|--------|
| **Авторизация** | JWT-токены, регистрация/вход, хэширование bcrypt |
| **Задачи** | Полный CRUD — создание, редактирование, удаление, смена статуса |
| **Статусы** | Три состояния: `Todo` → `В работе` → `Готово` |
| **Приоритеты** | Низкий / Средний / Высокий с визуальными метками |
| **Фильтрация** | По статусу, приоритету, поиск по названию и описанию |
| **Пагинация** | Серверная пагинация с метаданными |
| **Валидация** | На клиенте и сервере, ошибки по каждому полю |
| **Безопасность** | Helmet, CORS, Rate limiting, защита от SQL-инъекций |
| **Docker** | Полный Docker Compose для запуска одной командой |

---

## 🛠 Стек технологий

### Frontend (Клиент)
- **React 18** + **TypeScript** — типобезопасная архитектура компонентов
- **React Router v6** — клиентская маршрутизация с защищёнными маршрутами
- **Zustand** — лёгкое глобальное состояние (авторизация)
- **TanStack Query v5** — серверное состояние, кэширование, фоновое обновление
- **Axios** — HTTP-клиент с интерцепторами запросов/ответов
- **Tailwind CSS** — утилитарный подход к адаптивным стилям
- **react-hot-toast** — уведомления

### Backend (Сервер)
- **Node.js** + **Express.js** — REST API сервер
- **MySQL 8** + **mysql2** — реляционная БД с пулом соединений
- **JWT** (jsonwebtoken) — stateless-аутентификация
- **bcryptjs** — безопасное хэширование паролей (коэффициент 12)
- **express-validator** — декларативная валидация входных данных
- **Helmet** — заголовки безопасности HTTP
- **express-rate-limit** — защита от брутфорса

---

## 🚀 Быстрый старт

### Требования
- Node.js 18+ и npm
- MySQL 8.0 запущенный локально
- (Опционально) Docker + Docker Compose

### Вариант А — Ручная установка

**1. Клонируй репозиторий**
```bash
git clone https://github.com/your-username/fullstack-task-manager.git
cd fullstack-task-manager
```

**2. Настройка бэкенда**
```bash
cd backend
cp .env.example .env       # Копируй шаблон окружения
# Открой .env и введи данные MySQL
npm install
npm run migrate            # Создаёт таблицы в БД
npm run dev                # Запускается на :5000
```

**3. Настройка фронтенда**
```bash
cd frontend
npm install
npm run dev                # Запускается на :3000
```

**4. Открой в браузере**
```
http://localhost:3000
```

### Вариант Б — Docker (Рекомендуется)

```bash
git clone https://github.com/your-username/fullstack-task-manager.git
cd fullstack-task-manager

# Скопируй и настрой переменные окружения
cp .env.example .env

# Запусти всё одной командой
docker compose up --build

# Приложение доступно по адресам:
# Фронтенд  →  http://localhost:3000
# API       →  http://localhost:5000
# БД        →  localhost:3306
```

---

## 📐 Архитектура

```
fullstack-task-manager/
│
├── 🖥️  frontend/                    # SPA на React + TypeScript
│   └── src/
│       ├── components/
│       │   ├── auth/                # Страницы входа и регистрации
│       │   ├── tasks/               # Дашборд, список задач, форма задачи
│       │   ├── layout/              # Лейаут, защищённые маршруты, сайдбар
│       │   └── ui/                  # Переиспользуемые: Button, Input, Modal, Badge
│       ├── hooks/                   # useAuth, useTasks (React Query)
│       ├── services/                # api.ts — Axios клиент и все API-вызовы
│       ├── store/                   # authStore.ts (Zustand)
│       └── types/                   # Общие TypeScript-интерфейсы
│
└── ⚙️  backend/                     # API на Node.js + Express
    └── src/
        ├── config/                  # database.js, migrate.js
        ├── models/                  # UserModel.js, TaskModel.js (SQL-слой)
        ├── controllers/             # authController, taskController (HTTP-слой)
        ├── middleware/              # auth.js (JWT), validators.js, errorHandler.js
        ├── routes/                  # authRoutes.js, taskRoutes.js
        └── server.js                # Точка входа Express-приложения
```

### Паттерны проектирования
- **Repository Pattern** — Модели инкапсулируют весь SQL, контроллеры не пишут запросы напрямую
- **Цепочка middleware** — валидация → аутентификация → контроллер
- **Service Layer** — `api.ts` на фронтенде централизует все HTTP-вызовы
- **Custom Hooks** — `useTasks` / `useAuth` отделяют получение данных от UI

---

## 🔌 API Справочник

Базовый URL: `http://localhost:5000/api`

### Авторизация

| Метод | Путь | Авт. | Описание |
|-------|------|------|----------|
| `POST` | `/auth/register` | ❌ | Создать аккаунт |
| `POST` | `/auth/login` | ❌ | Войти, получить JWT |
| `GET` | `/auth/me` | ✅ | Текущий пользователь |

### Задачи

| Метод | Путь | Авт. | Описание |
|-------|------|------|----------|
| `GET` | `/tasks` | ✅ | Список задач (пагинация, фильтры) |
| `POST` | `/tasks` | ✅ | Создать задачу |
| `GET` | `/tasks/stats` | ✅ | Статистика по статусам |
| `GET` | `/tasks/:id` | ✅ | Получить одну задачу |
| `PATCH` | `/tasks/:id` | ✅ | Частично обновить задачу |
| `DELETE` | `/tasks/:id` | ✅ | Удалить задачу |

#### Query-параметры для `GET /tasks`
```
page      integer   Номер страницы (по умолчанию: 1)
limit     integer   Элементов на странице (по умолч.: 10, макс.: 100)
status    string    Фильтр: todo | in_progress | done
priority  string    Фильтр: low | medium | high
search    string    Поиск по заголовку и описанию
sort      string    Сортировка: created_at | due_date | priority | title
order     string    ASC | DESC (по умолчанию: DESC)
```

#### Примеры запросов

**Регистрация:**
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"Алекс","email":"alex@example.com","password":"Secret123"}'
```

**Создание задачи:**
```bash
curl -X POST http://localhost:5000/api/tasks \
  -H "Authorization: Bearer <ваш_jwt_токен>" \
  -H "Content-Type: application/json" \
  -d '{"title":"Сделать крутое приложение","priority":"high","due_date":"2026-03-01"}'
```

**Формат ответа:**
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

## 🐳 Docker Деплой

### Разработка
```bash
docker compose up
```

### Продакшн-сборка
```bash
# Фронтенд собирается в статику и раздаётся Nginx-ом
# Бэкенд работает в production-режиме без nodemon
docker compose -f docker-compose.yml up -d --build
```

### Полезные команды
```bash
docker compose logs -f backend        # Следить за логами API
docker compose exec db mysql -u root -p task_manager  # MySQL CLI
docker compose down -v                # Остановить и удалить volumes
```

---

## 🗄️ Схема базы данных

```sql
-- Таблица пользователей
users (
  id            INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  name          VARCHAR(100) NOT NULL,
  email         VARCHAR(255) NOT NULL UNIQUE,
  password_hash VARCHAR(255) NOT NULL,   -- Никогда не хранится в открытом виде
  created_at    TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at    TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
)

-- Таблица задач
tasks (
  id          INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  user_id     INT UNSIGNED NOT NULL,     -- FK → users.id (CASCADE DELETE)
  title       VARCHAR(255) NOT NULL,
  description TEXT,
  status      ENUM('todo','in_progress','done') DEFAULT 'todo',
  priority    ENUM('low','medium','high')       DEFAULT 'medium',
  due_date    DATE,
  position    INT UNSIGNED DEFAULT 0,
  created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
)
```

---

## 🔒 Безопасность

- **Пароли** хэшируются bcrypt (коэффициент 12) — никогда не хранятся в открытом виде
- **JWT** токены истекают через 7 дней, проверяются на каждом запросе
- **Изоляция данных** — каждый запрос привязан к `user_id`, пользователи не видят чужие данные
- **Rate limiting** — эндпоинты авторизации ограничены: 10 запросов / 15 минут
- **Валидация** — все входные данные проверяются до обращения к БД
- **Helmet** — автоматически устанавливает 11 заголовков безопасности HTTP
- **SQL-инъекции** исключены благодаря параметризованным запросам (`?` плейсхолдеры)

---

## 📝 Лицензия

MIT — свободное использование в личных и коммерческих проектах.

---

<div align="center">
  <sub>Создано с ❤️ как демонстрационный проект чистой архитектуры</sub>
</div>
