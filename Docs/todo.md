# Backlogr — Cursor Prompt Plan

Stack: Next.js 14 + TypeScript + App Router + Tailwind + Supabase  
Auth: Google OAuth

---

# PHASE 0 — Project Setup (MVP)

- [ ] Prompt: Generate Next.js 14 (App Router) project scaffold with TypeScript, Tailwind CSS, and Supabase integration.
- [ ] Prompt: Add Supabase environment config files and create `/lib/supabaseClient.ts`.
- [ ] Prompt: Install and configure Supabase Auth for Google OAuth.

---

# PHASE 1 — Authentication (MVP)

- [ ] Prompt: Create sign-in page with "Sign in with Google" button using Supabase Auth.
- [ ] Prompt: Implement Supabase OAuth callback logic to store authenticated session.
- [ ] Prompt: Create trigger to auto-insert profile on first login with default role `business`.
- [ ] Prompt: Implement logout and protected route middleware.

---

# PHASE 2 — Database & Schema (MVP)

- [ ] Prompt: Generate SQL for `profiles` table.
- [ ] Prompt: Generate SQL for `issues` table with defaults.
- [ ] Prompt: Test basic inserts and fetches.

---

# PHASE 3 — Row Level Security (MVP)

- [ ] Prompt: Create RLS for `profiles` (users read self, admin can read/update all).
- [ ] Prompt: Create RLS for `issues` (business: own issues, insert only; dev: all issues, update/assign).
- [ ] Prompt: Add JWT claim checks using `profiles.role`.

---

# PHASE 4 — Role-Based Routing (MVP)

- [ ] Prompt: Create `useUserProfile()` hook.
- [ ] Prompt: Redirect business → `/submit`, developer/admin → `/board`.
- [ ] Prompt: Block unauthorized routes.

---

# PHASE 5 — Business UI (Ticket Submission) (MVP)

- [ ] Prompt: Create `/submit` page with ticket submission form (title, description, priority).
- [ ] Prompt: Validate form and insert into Supabase.
- [ ] Prompt: Add list of previously submitted tickets by user.
- [ ] Prompt: Show status badges, sort newest first.

---

# PHASE 6 — Developer Board (Kanban) (MVP)

- [ ] Prompt: Create `/board` page with 4 columns (Backlog, Todo, In Progress, Done).
- [ ] Prompt: Fetch and group issues by status.
- [ ] Prompt: Create `IssueCard` component.
- [ ] Prompt: Implement drag & drop using dnd-kit / react-beautiful-dnd.
- [ ] Prompt: Update issue status in Supabase on drop.
- [ ] Prompt: Create modal with full issue details and edit functionality.

---

# PHASE 7 — Assignment & Developer Controls (MVP)

- [ ] Prompt: Add dropdown in issue modal to assign developer.
- [ ] Prompt: Fetch all developers for dropdown.
- [ ] Prompt: Update `assigned_to` in Supabase on change.

---

# PHASE 8 — Real-Time Updates (Optional MVP)

- [ ] Prompt: Subscribe to Supabase Realtime updates for issues.
- [ ] Prompt: Update board live on INSERT/UPDATE.
- [ ] Prompt: Cleanup subscription on unmount.

---

# PHASE 9 — Testing (MVP)

- [ ] Prompt: Generate test plan for ticket submission, board updates, assignment, and RLS enforcement.
- [ ] Prompt: Create Cypress / Playwright tests for `/submit` page.

---

# PHASE 10 — Admin Panel (Optional MVP)

- [ ] Prompt: Create `/admin` page to manage profiles and roles.
- [ ] Prompt: Update role in Supabase.
- [ ] Prompt: Restrict admin page to admin users only.

---

# PHASE 11 — Deployment (MVP)

- [ ] Prompt: Generate Vercel deployment config.
- [ ] Prompt: Add Supabase environment variables.
- [ ] Prompt: Test OAuth redirects and RLS in production.

---

# PHASE 12 — UX Enhancements (Post-MVP)

- [ ] Prompt: Add loading, error, and empty states.
- [ ] Prompt: Add toast notifications.
- [ ] Prompt: Make layout responsive.
- [ ] Prompt: Optional dark mode.

---

# PHASE 13 — v2 Enhancements (Future)

- [ ] Prompt: Add comments table and UI.
- [ ] Prompt: Add attachments with Supabase Storage.
- [ ] Prompt: Add email notifications.
- [ ] Prompt: Add Slack webhook integration.
- [ ] Prompt: Add activity log / audit history.

---

# PHASE 14 — Post-MVP Multi-Tenant / Team Creation

- [ ] Prompt: Create `teams` table with name, created_by, created_at.
- [ ] Prompt: Create `team_members` table (user_id, team_id, role).
- [ ] Prompt: Add `team_id` to `issues` table and enforce NOT NULL.
- [ ] Prompt: Update all issue queries to filter by active `team_id`.
- [ ] Prompt: Create `/create-team` page to allow authenticated users to create a team.
- [ ] Prompt: Auto-assign creator as `admin` of team.
- [ ] Prompt: Create team selection / switching in navbar.
- [ ] Prompt: Add invite by email flow to add team members.
- [ ] Prompt: Update RLS policies to enforce team membership and team-scoped roles.
- [ ] Prompt: Prevent cross-team visibility and access.

---

# PHASE 15 — SaaS / Future Enhancements

- [ ] Prompt: Add billing/subscription logic (Stripe).
- [ ] Prompt: Add usage limits (issues, seats).
- [ ] Prompt: Team settings page.
- [ ] Prompt: Multi-team audit logs.

---

# MVP Completion Criteria

- [ ] Google OAuth login works.
- [ ] Profiles auto-create.
- [ ] Business users can submit tickets.
- [ ] Developers can manage board.
- [ ] RLS correctly enforces access.
- [ ] Status updates persist.

---

# Post-MVP Completion Criteria

- [ ] Teams can be created by users.
- [ ] Users can join teams via invites.
- [ ] Team data is isolated.
- [ ] Roles are team-scoped.
- [ ] Multi-team queries correctly filter data.
