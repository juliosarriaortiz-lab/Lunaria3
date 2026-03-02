# Minecraft Discord Bot Dashboard

## Overview

This is a full-stack web application that serves as a dashboard for a Minecraft Discord bot. The bot runs on Discord and handles commands related to Minecraft server events, user registration (linking Discord accounts to Minecraft usernames), and role viewing. The web dashboard displays registered players, system status, and bot command references with a Minecraft-themed dark UI.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend
- **Framework**: React 18 with TypeScript, bundled via Vite
- **Routing**: Wouter (lightweight client-side router)
- **State Management / Data Fetching**: TanStack React Query for server state
- **UI Components**: shadcn/ui (new-york style) built on Radix UI primitives
- **Styling**: Tailwind CSS with CSS variables for theming, using a custom Minecraft-inspired dark theme (obsidian background, grass green primary, nether purple secondary, diamond blue accent)
- **Animations**: Framer Motion for card and page transitions
- **Fonts**: VT323 (pixel font for headings) and DM Sans (body text)
- **Path aliases**: `@/` maps to `client/src/`, `@shared/` maps to `shared/`

### Backend
- **Runtime**: Node.js with Express
- **Language**: TypeScript, executed via `tsx`
- **API Structure**: RESTful API under `/api/` prefix. Routes defined in `server/routes.ts`, with route schemas in `shared/routes.ts` using Zod for validation
- **Discord Bot**: Built with discord.js v14, runs alongside the Express server (started in `registerRoutes`). Handles commands like `!evento`, `!ver`, and `!registre`
- **Dev Server**: Vite dev server middleware integrated into Express for HMR during development
- **Production Build**: Vite builds the client to `dist/public`, esbuild bundles the server to `dist/index.cjs`

### Data Layer
- **Database**: PostgreSQL (required via `DATABASE_URL` environment variable)
- **ORM**: Drizzle ORM with `drizzle-zod` for schema-to-Zod integration
- **Schema Location**: `shared/schema.ts` — shared between client and server
- **Current Schema**: Single table `discord_users` with fields: `id` (serial PK), `discordId` (unique text), `minecraftNick` (text), `roles` (text array), `lastSeen` (timestamp)
- **Migrations**: Managed via `drizzle-kit push` (push-based, no migration files needed for dev)
- **Storage Pattern**: Repository pattern via `IStorage` interface in `server/storage.ts`, implemented by `DatabaseStorage` class

### Shared Code
- `shared/schema.ts` — Database schema and Zod types shared between frontend and backend
- `shared/routes.ts` — API route definitions with paths and response schemas, used by both server routes and client data fetching hooks

### Build System
- `script/build.ts` — Custom build script that runs Vite for client and esbuild for server
- Server dependencies are selectively bundled (allowlist pattern) to optimize cold start times
- Production output goes to `dist/` directory

## External Dependencies

### Required Services
- **PostgreSQL Database**: Connection via `DATABASE_URL` environment variable. Used for storing Discord user data. Sessions use `connect-pg-simple`
- **Discord Bot Token**: The bot uses `discord.js` and requires a bot token (likely via `DISCORD_TOKEN` or similar environment variable) to connect to Discord's gateway

### Key NPM Packages
- `discord.js` v14 — Discord bot framework
- `drizzle-orm` + `drizzle-kit` — Database ORM and migration tooling
- `express` — HTTP server
- `@tanstack/react-query` — Client-side data fetching
- `framer-motion` — Animations
- `zod` — Runtime type validation
- `wouter` — Client-side routing
- Full shadcn/ui component library (Radix UI primitives)
