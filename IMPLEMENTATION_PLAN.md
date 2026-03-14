# React + Supabase Mini App — Implementation Plan

## 1) Project Summary
Build a standalone portfolio project: a Task Tracker app using Vite + React + TypeScript + Supabase.
Core scope: magic-link auth, protected CRUD, row-level security (RLS), Vercel deployment, and portfolio case-study integration.

## 2) Goals / Success Criteria
- Users can sign in via Supabase email magic link.
- Authenticated users can create, read, update, and delete only their own tasks.
- RLS policies enforce per-user data access.
- App is deployed to Vercel with working production URL.
- Portfolio case study is updated with live app + GitHub links and real outcomes.

## 3) Stack Decisions
- Frontend: Vite + React + TypeScript
- Styling: clean custom CSS
- Backend: Supabase (new project)
- Auth: Supabase magic link
- Deploy: Vercel (main branch auto-deploy)

## 4) v1 Feature Scope
- Auth screens:
  - Sign in (email magic link)
  - Sign out
- Protected app area:
  - Task list
  - Create task
  - Edit task
  - Delete task
  - Update task status (`todo`, `in_progress`, `done`)
- UX states:
  - loading
  - empty state
  - error state

## 5) Data Model
Table: `public.tasks`
- `id` (uuid, primary key, default gen_random_uuid())
- `user_id` (uuid, not null, references auth.users(id))
- `title` (text, not null)
- `description` (text, nullable)
- `status` (text, not null, check in `todo|in_progress|done`, default `todo`)
- `created_at` (timestamptz, default now())
- `updated_at` (timestamptz, default now())

## 6) Security (RLS)
Enable RLS on `public.tasks` with policies:
- SELECT: user can read rows where `auth.uid() = user_id`
- INSERT: user can insert rows where `auth.uid() = user_id`
- UPDATE: user can update rows where `auth.uid() = user_id`
- DELETE: user can delete rows where `auth.uid() = user_id`

## 7) Environment Variables
- `VITE_SUPABASE_URL`
- `VITE_SUPABASE_ANON_KEY`

Local:
- `.env.local` with above keys

Vercel:
- same keys set in Project Settings -> Environment Variables

## 8) Implementation Plan
1. Scaffold repo with Vite React TS.
2. Install Supabase JS client.
3. Build auth/session provider and protected route logic.
4. Implement task CRUD UI and service layer.
5. Add RLS SQL migration and verify policies.
6. Add lint + typecheck + test baseline.
7. Deploy to Vercel and validate production auth flow.
8. Update portfolio case study page with real links and outcomes.

## 9) Test Plan
Automated:
- Lint passes.
- Typecheck passes.
- Core tests pass (auth guard + CRUD behavior).

Manual:
- Sign in via magic link works on local + prod.
- Authenticated user can CRUD tasks.
- User cannot access another user’s tasks.
- Reload preserves expected state.
- Error and empty states render correctly.

## 10) Out of Scope (v1)
- OAuth providers
- File uploads
- Team/shared tasks
- Push notifications

## 11) Deliverables
- Running mini app repo (public GitHub)
- Live Vercel URL
- SQL migration/RLS policies documented
- Updated portfolio case study page with:
  - problem
  - build scope
  - stack
  - outcomes
  - live demo link
  - GitHub link
