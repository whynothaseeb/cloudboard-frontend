# CloudBoard Frontend

CloudBoard is a production-grade Kanban-style task management frontend built with:

- React (Vite)
- Tailwind CSS
- React Router
- Context API
- Axios
- Docker
- Nginx
- CI/CD-ready architecture

This project demonstrates both **Software Engineering discipline** and **DevOps readiness**.

---

# Architecture Overview

The frontend follows strict separation of concerns:

- UI Components → Pure presentation
- Pages → Route-level composition
- Services → API abstraction layer
- Context → Global state management
- Routes → Centralized routing
- Infrastructure → Docker + deployment configuration

No business logic is mixed with UI rendering.

---

# Folder Structure

```
cloudboard-frontend/
│
├── public/
│   └── favicon.svg
│
├── src/
│   │
│   ├── assets/
│   │   └── logo.svg
│   │
│   ├── components/
│   │   ├── layout/
│   │   │   ├── Navbar.jsx
│   │   │   ├── Footer.jsx
│   │   │   └── Sidebar.jsx
│   │   │
│   │   ├── board/
│   │   │   ├── BoardColumn.jsx
│   │   │   ├── TaskCard.jsx
│   │   │   └── CreateTaskModal.jsx
│   │   │
│   │   └── ui/
│   │       ├── Button.jsx
│   │       ├── Input.jsx
│   │       ├── Modal.jsx
│   │       ├── Spinner.jsx
│   │       └── ProtectedRoute.jsx
│   │
│   ├── pages/
│   │   ├── Landing.jsx
│   │   ├── Login.jsx
│   │   ├── Register.jsx
│   │   ├── Dashboard.jsx
│   │   ├── Board.jsx
│   │   └── NotFound.jsx
│   │
│   ├── context/
│   │   └── AuthContext.jsx
│   │
│   ├── services/
│   │   ├── api.js
│   │   ├── authService.js
│   │   └── boardService.js
│   │
│   ├── hooks/
│   │   └── useAuth.js
│   │
│   ├── routes/
│   │   └── AppRoutes.jsx
│   │
│   ├── utils/
│   │   └── constants.js
│   │
│   ├── App.jsx
│   ├── main.jsx
│   └── index.css
│
├── .env
├── .env.example
├── .gitignore
├── Dockerfile
├── nginx.conf
├── package.json
├── tailwind.config.js
├── postcss.config.js
└── vite.config.js
```

---

# Root-Level Files

## package.json
Defines:
- Project metadata
- Dependencies
- Scripts (`dev`, `build`, `preview`)
- Version management

---

## vite.config.js
Vite configuration file.

Controls:
- Dev server
- Build output
- Environment handling

---

## tailwind.config.js
Tailwind configuration.

Defines:
- Content paths
- Theme customization
- Dark mode strategy
- Plugins

---

## postcss.config.js
Configures:
- Tailwind processing
- Autoprefixer

---

## .env
Environment variables (NOT committed).

Example:
```
VITE_API_URL=http://localhost:8000/api
```

---

## .env.example
Template for environment variables.

---

## .gitignore
Prevents committing:
- node_modules
- dist
- .env
- local configs

---

## Dockerfile
Multi-stage build:

1. Node build stage
2. Nginx production stage

Used for:
- Docker builds
- Kubernetes deployment

---

## nginx.conf
Nginx configuration for SPA routing.

Handles:
- React Router fallback
- Static file serving
- Performance caching

---

# src Directory

## main.jsx
Application entry point.

Responsibilities:
- Mount React
- Wrap providers
- Attach Router
- Attach Auth context

No business logic.

---

## App.jsx
Top-level layout component.

Responsibilities:
- Layout shell (Navbar + Footer)
- Theme control
- Inject `AppRoutes`

No routing logic directly inside.

---

# src/routes

## AppRoutes.jsx
Centralized route definitions.

Defines:
- Public routes
- Protected routes
- 404 route

Keeps routing separate from layout.

---

# src/context

## AuthContext.jsx
Global authentication state.

Handles:
- User state
- Login
- Logout
- Token storage logic

---

# src/hooks

## useAuth.js
Custom hook wrapper around AuthContext.

Purpose:
- Cleaner component usage
- Encapsulation of context access

---

# src/services

All backend communication lives here.

Components NEVER call axios directly.

---

## api.js
Axios instance configuration.

Handles:
- Base URL
- JWT injection
- Response interceptors
- Global error handling

---

## authService.js
Authentication API calls:

- login()
- register()
- logout()

Pure API wrapper.

---

## boardService.js
Board-related API calls:

- getBoards()
- createBoard()
- getBoardById()
- createTask()
- updateTask()
- deleteTask()

No UI logic.

---

# src/utils

## constants.js
Application-wide constants:

- API endpoints
- Status labels
- Column types
- Role identifiers

Prevents magic strings.

---

# src/components

## components/layout

### Navbar.jsx
Top navigation bar.

Contains:
- Logo
- Navigation links
- Auth buttons
- Dark mode toggle

---

### Footer.jsx
Footer with:
- Branding
- Version
- Links

---

### Sidebar.jsx
Dashboard navigation sidebar.

Used in:
- Dashboard
- Board pages

---

## components/ui

Reusable UI primitives.

These are intentionally dumb components.

---

### Button.jsx
Reusable styled button.

Props:
- variant
- size
- loading
- disabled

---

### Input.jsx
Reusable input field.

Handles:
- Label
- Error display
- Controlled input

---

### Modal.jsx
Generic modal wrapper.

Used for:
- Create board
- Create task
- Confirm deletion

---

### Spinner.jsx
Loading indicator component.

---

### ProtectedRoute.jsx
Route guard wrapper.

Checks:
- Auth token
- Redirect logic

Used inside routing layer.

---

## components/board

### BoardColumn.jsx
Represents:
- One Kanban column
- Contains TaskCard components

---

### TaskCard.jsx
Represents:
- Single task item
- Clickable for editing

---

### CreateTaskModal.jsx
Modal form for:
- Creating new tasks
- Editing tasks

---

# src/pages

Route-level components.

Pages compose layout + services + UI components.

---

## Landing.jsx
Public homepage.

Includes:
- App introduction
- CTA buttons

---

## Login.jsx
Login page.

Handles:
- Controlled form
- API call via authService
- Context login
- Redirect

---

## Register.jsx
Registration page.

Handles:
- Form validation
- API call

---

## Dashboard.jsx
User overview page.

Displays:
- List of boards
- Create board option

---

## Board.jsx
Main Kanban page.

Displays:
- Columns
- Tasks

---

## NotFound.jsx
404 fallback page.

---

# Running the Project

Install dependencies:
```
npm install
```

Start development server:
```
npm run dev
```

Build production:
```
npm run build
```

Preview production build:
```
npm run preview
```

---

# Docker Usage

Build image:
```
docker build -t cloudboard-frontend .
```

Run container:
```
docker run -p 80:80 cloudboard-frontend
```

---

# DevOps Considerations

This project is designed to:

- Be containerized
- Deploy behind Nginx
- Integrate with Kubernetes
- Use environment-based configuration
- Support CI pipelines (GitHub Actions)

---

# Future Enhancements

- Drag & Drop (dnd-kit)
- Role-based access
- Token refresh strategy
- Toast notifications
- E2E testing (Cypress)
- Unit testing (Vitest)

---

# Purpose

This frontend demonstrates:

- Clean frontend architecture
- Scalable folder structure
- API abstraction discipline
- Production readiness
- DevOps awareness

Suitable for:

- Software Engineering portfolio
- DevOps portfolio
- Full-stack interviews