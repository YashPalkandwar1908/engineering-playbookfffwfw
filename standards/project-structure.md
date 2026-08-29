# Project Structure

A simple, consistent structure for keeping projects easy to understand, maintain, and grow.

## Default Structure

```text
project-name/
│
├── README.md
├── SECURITY.md          # When security matters
├── .gitignore
├── .env.example         # When needed
│
├── src/
├── tests/
├── docs/
└── scripts/
```

## What Goes Where

| Location       | Purpose                                        |
| -------------- | ---------------------------------------------- |
| `README.md`    | Project overview, setup, and usage             |
| `SECURITY.md`  | Security practices and vulnerability reporting |
| `.gitignore`   | Files Git should ignore                        |
| `.env.example` | Example environment configuration              |
| `src/`         | Main application/source code                   |
| `tests/`       | Automated tests                                |
| `docs/`        | Detailed project documentation                 |
| `scripts/`     | Development and maintenance scripts            |

`SECURITY.md` and `.env.example` are **conditional**. Add them when the project needs them.

## Frontend & Backend

For full-stack projects, separate frontend and backend code:

```text
src/
│
├── frontend/
│   ├── components/
│   ├── pages/
│   ├── services/
│   └── ...
│
└── backend/
    ├── handlers/
    ├── services/
    ├── repositories/
    ├── models/
    └── ...
```

Not every project needs both.

```text
Frontend only  → src/frontend/
Backend only   → src/backend/
Simple project → src/...
```

## Organize by Responsibility

Keep related responsibilities separated.

```text
Handler
   ↓
Service
   ↓
Repository
   ↓
Database
```

For frontend:

```text
Page
  ↓
Component
  ↓
Service / API
```

This keeps code easier to understand, test, review, and modify — especially when using AI-assisted development.

> **Don't split code just because a file reaches a certain number of lines. Split it when it has too many responsibilities or becomes difficult to work with.**

## Add When Needed

Projects can add folders based on their actual requirements:

```text
config/
data/
examples/
assets/
migrations/
models/
.github/
```

These are **not required by default**.

## Growing Projects

Start simple and expand as the project grows.

```text
Small Project
     │
     ▼
Basic Structure
     │
     ▼
Project Grows
     │
     ▼
Add Structure When Needed
     │
     ▼
Mature Project
```

Don't create folders just because a template includes them.

## Core Rule

> **Keep the structure simple, predictable, and appropriate for the project's size and purpose.**

The goal is **consistency without unnecessary complexity**.
