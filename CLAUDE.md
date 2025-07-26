# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an AI chatbot application built with Next.js 15, the AI SDK, and Vercel's stack. It's a full-featured chat application with authentication, file uploads, artifacts (code/text/image/sheet editing), and conversation history.

## Development Commands

```bash
# Development
pnpm dev              # Start development server with turbo
pnpm build            # Run migrations and build for production
pnpm start            # Start production server

# Database
pnpm db:generate      # Generate new migrations
pnpm db:migrate       # Run pending migrations
pnpm db:studio        # Open Drizzle Studio
pnpm db:push          # Push schema changes
pnpm db:pull          # Pull schema from database

# Code Quality
pnpm lint             # Run ESLint and Biome linting
pnpm lint:fix         # Fix linting issues automatically
pnpm format           # Format code with Biome

# Testing
pnpm test             # Run Playwright tests (sets PLAYWRIGHT=True env)
```

## Architecture

### Directory Structure
- `app/` - Next.js App Router with route groups:
  - `(auth)/` - Authentication pages and API routes
  - `(chat)/` - Chat interface and API routes
- `artifacts/` - Artifact creation and management (code, text, image, sheet)
- `components/` - React components including UI primitives from shadcn/ui
- `lib/` - Core business logic:
  - `ai/` - AI provider configuration, models, prompts, and tools
  - `db/` - Database schema, queries, and migrations (Drizzle ORM)
  - `editor/` - Rich text editor configuration and utilities
- `hooks/` - Custom React hooks
- `tests/` - Playwright end-to-end and route tests

### Key Technologies
- **Next.js 15** with App Router and React Server Components
- **AI SDK** with support for multiple providers (xAI default, OpenAI, etc.)
- **Drizzle ORM** with PostgreSQL (Neon Serverless)
- **Auth.js** for authentication with credentials and guest mode
- **shadcn/ui** components with Radix UI primitives
- **Tailwind CSS** for styling
- **Biome** for formatting and linting
- **Playwright** for testing

### Database Schema
- `User` - User accounts with email/password
- `Chat` - Conversations with visibility settings
- `Message_v2` - Chat messages with parts and attachments (replaces deprecated Message)
- `Vote_v2` - Message voting system (replaces deprecated Vote)
- `Document` - Artifacts (text, code, image, sheet)
- `Suggestion` - Document editing suggestions
- `Stream` - Chat streaming sessions

### Authentication
- Supports both regular users (email/password) and guest users
- Guest users are created automatically and stored in database
- Uses NextAuth.js with custom credentials provider

### AI Integration
- Model abstraction in `lib/ai/models.ts` with configurable chat models
- Custom provider setup in `lib/ai/providers.ts`
- Tools for document creation, weather, and suggestions
- Streaming responses with the AI SDK

### Artifacts System
- Four types: text, code, image, sheet
- Each type has client and server components in `artifacts/`
- Documents stored in database with suggestions for collaborative editing

## Testing

The project uses Playwright for end-to-end testing:
- Tests split into `e2e/` and `routes/` projects
- Automatic dev server startup during tests
- 240-second timeout for long-running operations
- Tests should pass before deployment

## Environment Setup

Set up environment variables in `.env.local`:
- Database: `POSTGRES_URL`
- Auth: `AUTH_SECRET`
- AI providers as needed

Run `vercel env pull` if using Vercel for deployment.

## Code Conventions

- Uses Biome for formatting and linting with specific rules in `biome.jsonc`
- TypeScript strict mode enabled
- Server Actions for mutations, React Server Components where possible
- Tailwind CSS with custom configurations
- Import from `@/` path alias for absolute imports