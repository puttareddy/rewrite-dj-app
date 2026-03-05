# Modernization Constitution

## Project Identity
- **Project Name:** AI Agent Platform
- **Source Path:** /Users/puttaiaharugunta/codebase/hachi/ai-agent-platform/rewrite-dj-app
- **Strategy:** new_build

## Governing Principles
1. **Functional Completeness** — All specified features must be implemented with full functionality
2. **Data Integrity** — All data must be stored in a persistent database, never in-memory
3. **Code Quality** — Modern patterns, clean code, proper error handling, and comprehensive testing
4. **Incremental Verification** — Each feature must be independently testable and verifiable
5. **No Mock Data** — Production code must never use mock data or in-memory stores

## Current State
- **Languages:** None (new project)
- **Frontend:** None
- **Backend:** None
- **Database:** None
- **Styling:** None

## Target State
- **Frontend:** Next.js 14 (App Router)
- **Backend:** Next.js 14 (API Routes)
- **Database:** PostgreSQL with Prisma ORM
- **Styling:** Tailwind CSS

## Migration Boundaries
- **MUST Preserve:**
  - None (new project build)
- **MAY Change:**
  - All architecture, patterns, and naming conventions are flexible
- **WILL Drop:**
  - None (new project build)

## Agent Guidelines
- Agents must implement all features from scratch according to specifications
- Each feature must be verified end-to-end with browser automation
- Database operations must use Prisma ORM with PostgreSQL
- API endpoints must follow REST conventions
- All data must persist across server restarts (no in-memory stores)
- Infrastructure features (0-4) must pass before any functional features
- Features must include proper error handling and user feedback
- All UI components must be responsive and accessible
