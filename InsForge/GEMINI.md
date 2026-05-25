# InsForge Project Context

InsForge is an all-in-one, open-source backend platform (BaaS) specifically designed for agentic coding. It provides AI coding agents with the necessary primitives—database, auth, storage, compute, and AI gateway—to build and deploy full-stack applications autonomously.

## Architecture Overview

The project is organized as a monorepo using **Turborepo**.

### Core Components
- **`backend/`**: Express.js server (Node.js) written in TypeScript with ESM. It handles API routing, authentication, and orchestrates services like PostgreSQL, S3 storage, and Deno edge functions.
- **`frontend/`**: A React + Vite shell that hosts the admin dashboard. It primarily mounts the `@insforge/dashboard` package.
- **`packages/dashboard/`**: The core logic, routes, and UI of the InsForge dashboard.
- **`packages/shared-schemas/`**: Centralized Zod schemas and TypeScript types used for validation and contracts across the monorepo.
- **`packages/ui/`**: A library of reusable design system primitives and components.
- **`openapi/`**: API specifications (YAML) for all InsForge services.
- **`functions/`**: Templates and examples for serverless edge functions (Deno).

## Tech Stack
- **Language**: TypeScript
- **Backend**: Node.js (Express), ESM, `tsx` (dev), `tsup` (build).
- **Frontend**: React, Vite, Tailwind CSS 4.
- **Database**: PostgreSQL (relational), `node-pg-migrate` for migrations.
- **Edge Runtime**: Deno (for serverless functions).
- **Monorepo Tooling**: Turborepo, npm workspaces.
- **Testing**: Vitest (unit/integration), Playwright (E2E).

## Key Commands

### Root Commands
- `npm run dev`: Starts all services in development mode.
- `npm run build`: Builds all packages and applications.
- `npm test`: Runs the full test suite across the monorepo.
- `npm run lint`: Runs ESLint and Prettier checks.
- `npm run typecheck`: Performs TypeScript type checking across all packages.

### Backend Specific (`backend/`)
- `npm run migrate:up:local`: Runs database migrations against the local instance defined in `.env`.
- `npm run test:e2e`: Runs end-to-end tests.

## Development Conventions

### General
- **Monorepo Boundaries**: Respect package boundaries. Contract changes must start in `packages/shared-schemas/`.
- **Validation**: Use Zod schemas from `@insforge/shared-schemas` for all API request/response validation.
- **Git**: Use Conventional Commits and branch prefixes (`feat/`, `fix/`, `docs/`, `refactor/`).

### Backend
- **ESM Imports**: Always use the `.js` extension in import specifiers (e.g., `import { userSchema } from './user.js'`).
- **Path Aliases**: Use the `@/` prefix to reference the `src/` directory (e.g., `import { logger } from '@/utils/logger.js'`).
- **Response Format**: Return raw JSON objects (not wrapped in a `data` key) for success responses.
- **Error Handling**: Use the `AppError` class and `errorMiddleware`.
- **Testing**: Backend tests in Vitest are configured to run sequentially (`singleFork: true`) to avoid database conflicts.

### Dashboard / Frontend
- **Data Fetching**: Use `apiClient` and React Query for all data operations.
- **Component Styling**: Prefer the primitives in `packages/ui/` and Tailwind CSS.

## MCP & Agent Integration
InsForge is designed to be controlled by AI agents via:
- **MCP Server**: Exposes tools for managing database, storage, functions, and more.
- **CLI & Skills**: Specifically for cloud-hosted environments.

When working with InsForge as an agent, prioritize using the available MCP tools or CLI to inspect state and logs before making changes.
