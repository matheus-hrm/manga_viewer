# manga_viewer

Web application for browsing and reading manga, built on top of the [MangaDex public API](https://api.mangadex.org).

## Overview

The application fetches manga listings and cover images directly from MangaDex. It supports search by title, displays curated lists ordered by rating and follow count, and provides per-chapter reading pages. Authentication is handled via Discord OAuth and persisted with NextAuth session tokens stored in a local SQLite database.

## Stack

| Dependency | Version | Role |
|---|---|---|
| Next.js | 13 | React framework with App Router and server components |
| NextAuth.js | 4 | Authentication with Discord OAuth provider |
| Prisma | 5 | ORM; manages the SQLite database for sessions and users |
| tRPC | 10 | End-to-end type-safe API layer between client and server |
| Tanstack Query | 4 | Client-side data fetching and caching |
| Tailwind CSS | 3 | Utility-first styling |
| Axios | 1 | HTTP client used in server-side API calls to MangaDex |
| Zod | 3 | Runtime schema validation for environment variables and tRPC inputs |
| TypeScript | 5 | Static typing across the entire codebase |

## Project structure

```
src/
  app/
    api/            # MangaDex API integration (search, listing, cover resolution)
    components/     # Shared UI components (Header, MangaList, SearchBar, etc.)
    (auth)/         # Login and register pages
    [id]/           # Dynamic route for manga detail and chapter reading
  env.mjs           # Zod-validated environment variable schema
  middleware.ts     # NextAuth session middleware
prisma/
  schema.prisma     # Database schema (User, Account, Session, VerificationToken)
```

## Environment variables

Copy `.env.example` to `.env` and fill in the required values:

```
DATABASE_URL="file:./db.sqlite"
NEXTAUTH_SECRET=""        # generate with: openssl rand -base64 32
NEXTAUTH_URL="http://localhost:3000"
DISCORD_CLIENT_ID=""
DISCORD_CLIENT_SECRET=""
```

`DISCORD_CLIENT_ID` and `DISCORD_CLIENT_SECRET` are obtained from the [Discord Developer Portal](https://discord.com/developers/applications). The redirect URI to register is `http://localhost:3000/api/auth/callback/discord`.

## Setup

```bash
npm install
npx prisma db push
npm run dev
```

The application will be available at `http://localhost:3000`.

## Scripts

| Command | Description |
|---|---|
| `npm run dev` | Start the development server |
| `npm run build` | Build for production |
| `npm run start` | Start the production server |
| `npm run db:push` | Apply the Prisma schema to the database |
| `npm run db:studio` | Open Prisma Studio to inspect the database |
| `npm run lint` | Run ESLint |

