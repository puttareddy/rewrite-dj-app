# Modernization Specification

## Overview

The AI Agent Platform is a comprehensive full-stack web application designed to manage AI agents, their configurations, and monitor their execution. The platform provides a modern, user-friendly interface for creating, configuring, and managing AI agents with real-time monitoring capabilities.

This modernization builds a new AI Agent Platform from scratch using a modern technology stack: Next.js 14 with App Router for both frontend and backend, PostgreSQL for persistent data storage, Prisma ORM for database interactions, and Tailwind CSS for styling.

## Current Architecture

There is no existing architecture - this is a new project build. The codebase consists only of template files and configuration for the InfiniOS autonomous development framework.

## Target Architecture

The AI Agent Platform will follow a monolithic full-stack architecture:

- **Frontend:** Next.js 14 with React 18 using the App Router pattern
- **Backend:** Next.js API Routes (serverless functions) with Prisma ORM
- **Database:** PostgreSQL with Prisma as the ORM layer
- **Styling:** Tailwind CSS with responsive design patterns
- **Authentication:** Session-based authentication with secure password hashing
- **API Style:** RESTful API endpoints

The application will be organized with:
- `/app/` - Next.js App Router pages and layouts
- `/app/api/` - API route handlers
- `/components/` - Reusable React components
- `/lib/` - Utility functions and configurations
- `/prisma/` - Database schema and migrations

## Data Models

### User
- `id`: UUID (primary key)
- `email`: String (unique, indexed)
- `password_hash`: String
- `name`: String
- `role`: Enum (admin, user)
- `created_at`: DateTime
- `updated_at`: DateTime

### Agent
- `id`: UUID (primary key)
- `name`: String
- `description`: Text (optional)
- `model_type`: Enum (gpt-4, gpt-3.5-turbo, claude-3, custom)
- `system_prompt`: Text
- `temperature`: Decimal (default: 0.7)
- `max_tokens`: Integer (default: 2048)
- `is_active`: Boolean (default: true)
- `user_id`: UUID (foreign key to User)
- `created_at`: DateTime
- `updated_at`: DateTime

### AgentExecution
- `id`: UUID (primary key)
- `agent_id`: UUID (foreign key to Agent)
- `prompt`: Text
- `response`: Text
- `tokens_used`: Integer
- `execution_time_ms`: Integer
- `status`: Enum (pending, running, completed, failed)
- `error_message`: Text (optional)
- `created_at`: DateTime
- `completed_at`: DateTime (optional)

### AgentConfig
- `id`: UUID (primary key)
- `agent_id`: UUID (foreign key to Agent)
- `key`: String
- `value`: String
- `created_at`: DateTime
- `updated_at`: DateTime

### APIKey
- `id`: UUID (primary key)
- `name`: String
- `key_hash`: String (hashed API key)
- `user_id`: UUID (foreign key to User)
- `is_active`: Boolean (default: true)
- `created_at`: DateTime
- `last_used_at`: DateTime (optional)

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register a new user
- `POST /api/auth/login` - Login with email/password
- `POST /api/auth/logout` - Logout current session
- `GET /api/auth/me` - Get current user info

### Users
- `GET /api/users` - List all users (admin only)
- `GET /api/users/:id` - Get user details
- `PUT /api/users/:id` - Update user (own profile)
- `DELETE /api/users/:id` - Delete user (admin only)

### Agents
- `GET /api/agents` - List all agents for current user
- `GET /api/agents/:id` - Get agent details
- `POST /api/agents` - Create new agent
- `PUT /api/agents/:id` - Update agent
- `DELETE /api/agents/:id` - Delete agent
- `GET /api/agents/:id/executions` - List executions for agent

### Agent Executions
- `GET /api/executions` - List all executions for current user
- `GET /api/executions/:id` - Get execution details
- `POST /api/executions` - Create and run new execution
- `DELETE /api/executions/:id` - Cancel execution (if pending/running)

### Agent Configs
- `GET /api/agents/:id/configs` - List configs for agent
- `POST /api/agents/:id/configs` - Add config to agent
- `PUT /api/agents/:id/configs/:configId` - Update config
- `DELETE /api/agents/:id/configs/:configId` - Delete config

### API Keys
- `GET /api/keys` - List all API keys for current user
- `POST /api/keys` - Create new API key
- `DELETE /api/keys/:id` - Revoke API key

## Business Rules

1. **User Authentication**
   - Users must register with a valid email and password
   - Passwords must be at least 8 characters
   - Passwords must be hashed using bcrypt
   - Session management for authenticated users
   - Admin role can access all users' data

2. **Agent Ownership**
   - Each agent is owned by exactly one user
   - Users can only view and edit their own agents
   - Admins can view and manage all agents

3. **Agent Execution**
   - Executions are tracked with status (pending, running, completed, failed)
   - Executions record token usage and execution time
   - Failed executions store error messages
   - Users can only see their own agent executions

4. **API Key Management**
   - API keys are hashed before storage
   - API keys can be revoked (deactivated)
   - Last used timestamp is updated on each use

5. **Configuration Management**
   - Each agent can have multiple key-value configurations
   - Configurations support dynamic parameter storage

## UI Components

### Authentication Screens
- **Login Page** - Email/password login form
- **Register Page** - User registration with password validation

### Dashboard
- **Dashboard Home** - Overview of agents, recent executions, and statistics
- **Stats Cards** - Agent count, execution count, tokens used, success rate

### Agent Management
- **Agent List** - Table/grid of all user's agents with status indicators
- **Agent Create Form** - Form for creating new agents
- **Agent Edit Form** - Form for editing existing agent details
- **Agent Detail View** - Full agent information with configuration panel
- **Agent Config Panel** - Add/edit/delete key-value configurations

### Execution Management
- **Execution List** - Filterable list of all executions
- **Execution Detail** - Full execution details with prompt/response
- **Execution History** - Historical view of agent executions

### API Key Management
- **API Key List** - Table of API keys with visibility options
- **API Key Create Modal** - Generate new API key with name
- **API Key Revoke** - Confirm and revoke API key

### Navigation
- **Top Navigation Bar** - Logo, navigation links, user menu
- **Sidebar Navigation** - Main navigation menu
- **Breadcrumbs** - Page navigation hierarchy

## Authentication & Authorization

### Current
- None (new project)

### Target
- **Session-based Authentication** using NextAuth.js (or custom implementation)
- **Password Hashing** using bcrypt
- **Protected Routes** using middleware
- **Role-based Access Control** (admin, user)
- API authentication via Bearer tokens (for API key usage)

## External Dependencies

### Development Dependencies
- Next.js 14 (React framework)
- React 18 (UI library)
- Prisma (Database ORM)
- PostgreSQL (Database)
- Tailwind CSS (Styling)
- bcrypt (Password hashing)
- uuid (UUID generation)
- zod (Schema validation)

### API Services
- OpenAI API (for GPT model integration - optional)
- Anthropic API (for Claude model integration - optional)

### Build Tools
- TypeScript (Type safety)
- ESLint (Code linting)
- Prettier (Code formatting)
