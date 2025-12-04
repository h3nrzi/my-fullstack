# Nx Monorepo Setup Guide

This document captures all steps we have completed so far while building the **NestJS + React Nx Monorepo**.

## 1. Create Nx Workspace

- Ran: `npx create-nx-workspace@latest my-fullstack`
- Selected stack: **none** (empty monorepo)
- Enabled Prettier: **Yes**
- CI Provider: **Do it later**
- Remote caching: **No**

## 2. Install Plugins

Installed the necessary Nx plugins:

```
npm install -D @nx/react @nx/nest
```

## 3. Generate NestJS Backend

Ran:

```
nx generate @nx/nest:application backend
```

Selections:

- Linter: **eslint**
- Unit test runner: **jest**

Generated backend at:

```
backend/
backend-e2e/
```

## 4. Generate React Frontend

Ran:

```
nx generate @nx/react:application frontend
```

Selections:

- Styles: **Tailwind**
- Routing: **Yes**
- Bundler: **Vite**
- Linter: **eslint**
- Unit tests: **vitest**
- E2E: **Playwright**
- Port: **4200**

Generated frontend at:

```
frontend/
frontend-e2e/
```

## 5. Add Workspaces Configuration

Updated `package.json` to include:

```
"private": true,
"workspaces": [
  "backend",
  "backend-e2e",
  "frontend",
  "frontend-e2e",
  "packages/*"
]
```

Then ran:

```
npm install
```

## 6. Create Shared Library (api-interfaces)

Ran:

```
nx generate @nx/js:library api-interfaces --directory=packages
```

Selections:

- Bundler: **none**
- Linter: **eslint**
- Tests: **none**

Generated shared library at:

```
packages/api-interfaces/
```

This library is intended to hold shared TypeScript interfaces and DTOs used by both backend and frontend.

---

This document will continue to grow as we build more features into the monorepo.
