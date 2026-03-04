# Backlogr — Cursor Prompt Workflow

Stack: Next.js 14 + TypeScript + App Router + Tailwind + Supabase  
Auth: Google OAuth  

Each prompt below is copy-paste ready for Cursor.  
Mark each prompt as complete as you go.

---

# PHASE 0 — Project Setup (MVP)

## Prompt 0.1 — Scaffold Project
```

Generate a Next.js 14 project with:

* TypeScript
* App Router
* Tailwind CSS configured
* Supabase JS client installed
  Output: folder/file structure only

```

## Prompt 0.2 — Supabase Client
```

Create `/lib/supabaseClient.ts` for Supabase integration.
Use placeholders for:

* NEXT_PUBLIC_SUPABASE_URL
* NEXT_PUBLIC_SUPABASE_ANON_KEY
  Include TypeScript types

```

## Prompt 0.3 — Supabase Auth Setup
```

Install and configure Supabase Auth.
Enable Google OAuth.
Generate example redirect handling code in Next.js.

```

---

# PHASE 1 — Authentication (MVP)

## Prompt 1.1 — Sign-in Page
```

Create `/login` page with a "Sign in with Google" button.
Use Supabase Auth client.
Include redirect after login.

```

## Prompt 1.2 — OAuth Callback
```

Generate Next.js server action or API route that handles Supabase OAuth callback.
Ensure session is stored client-side.

```

## Prompt 1.3 — Auto-create Profile
```

Add trigger or server-side logic to insert user into `profiles` table on first login.
Default role: business

```

## Prompt 1.4 — Protected Routes
```

Create route guard or layout wrapper that:

* Blocks unauthenticated users
* Redirects based on role:

  * business → /submit
  * developer/admin → /board

```

---

# PHASE 2 — Database & Schema (MVP)

## Prompt 2.1 — Profiles Table
```

Generate SQL for `profiles` table:

* id (uuid, references auth.users)
* email
* full_name
* role (business | developer | admin)
* created_at

```

## Prompt 2.2 — Issues Table
```

Generate SQL for `issues` table:

* id (uuid)
* title
* description
* status (backlog | todo | in_progress | done) default backlog
* priority (low | medium | high) default medium
* created_by (uuid → profiles.id)
* assigned_to (uuid → profiles.id, nullable)
* created_at
* updated_at

```

---

# PHASE 3 — Row Level Security (MVP)

## Prompt 3.1 — Profiles RLS
```

Create RLS policies:

* Users can read their own profile
* Admin can read/update all profiles

```

## Prompt 3.2 — Issues RLS
```

Create RLS policies:

* Business:

  * Insert issues
  * Read own issues
  * Cannot update status
* Developer:

  * Read all issues
  * Update status
  * Assign issues
    Use JWT claim for role

```

---

# PHASE 4 — Role-Based Routing (MVP)

## Prompt 4.1 — Hook & Redirect
```

Create `useUserProfile()` hook to fetch profile.
Redirect based on role:

* business → /submit
* developer/admin → /board
  Block unauthorized routes

```

---

# PHASE 5 — Business UI (Ticket Submission)

## Prompt 5.1 — Submit Form
```

Create `/submit` page:

* Fields: title, description, priority
* Validation: title & description required, priority default medium
* Insert into Supabase on submit

```

## Prompt 5.2 — My Tickets List
```

Add section to `/submit` page showing tickets created by current user

* Include status badge
* Sort newest first

```

---

# PHASE 6 — Developer Board (Kanban)

## Prompt 6.1 — Board Layout
```

Create `/board` page with 4 columns: Backlog, Todo, In Progress, Done

* Use Tailwind CSS
* Fetch issues grouped by status

```

## Prompt 6.2 — Issue Card
```

Create `IssueCard` component:

* Display title, priority, assigned_to
* Open modal on click

```

## Prompt 6.3 — Drag & Drop
```

Implement drag & drop for status change

* Use react-beautiful-dnd or dnd-kit
* Update status in Supabase on drop
* Optimistic UI updates

```

## Prompt 6.4 — Issue Modal
```

Modal displays:

* Full title
* Description
* Priority
* Assigned_to
* Created_by
* Allows developers to edit status, priority, assign dev

```

---

# PHASE 7 — Assignment (MVP)

## Prompt 7.1 — Developer Dropdown
```

Fetch all profiles where role = developer
Populate assign dropdown in modal
Update assigned_to in Supabase

```

---

# PHASE 8 — Real-Time Updates (Optional MVP)

## Prompt 8.1 — Realtime Board
```

Subscribe to Supabase realtime on `issues`
Update board on INSERT and UPDATE
Cleanup subscription on unmount

```

---

# PHASE 9 — Testing (MVP)

## Prompt 9.1 — Test Plan
```

Generate test plan for:

* Ticket submission
* Developer board updates
* Assignment
* RLS enforcement

```

## Prompt 9.2 — Automation
```

Create Cypress/Playwright tests for `/submit` and `/board` flows

```

---

# PHASE 10 — Admin Panel (Optional MVP)

## Prompt 10.1 — Admin Page
```

Create `/admin` page:

* Display all profiles
* Allow role changes
* Restrict page to admin users

```

---

# PHASE 11 — Deployment (MVP)

## Prompt 11.1 — Vercel Config
```

Generate `vercel.json` for deployment
Include Supabase env vars

```

## Prompt 11.2 — Production Test
```

Test OAuth login and RLS in production environment

```

---

# PHASE 12 — UX Enhancements

- Loading states
- Error states
- Toast notifications
- Empty states
- Responsive layout
- Optional dark mode

---

# PHASE 13 — Post-MVP Multi-Tenant / Team Creation

## Prompt 13.1 — Teams Table
```

Create `teams` table:

* id (uuid)
* name
* created_by (uuid → profiles.id)
* created_at

```

## Prompt 13.2 — Team Members Table
```

Create `team_members` table:

* id (uuid)
* team_id (uuid → teams.id)
* user_id (uuid → profiles.id)
* role (business | developer | admin)
* created_at

```

## Prompt 13.3 — Issue Table Update
```

Add `team_id` (NOT NULL) to `issues`
Update all issue queries to filter by active team

```

## Prompt 13.4 — Team Creation Flow
```

Create `/create-team` page:

* Authenticated users can create team
* Creator becomes admin of team
* Redirect to team dashboard

```

## Prompt 13.5 — Team Switching
```

Add team selector in navbar
Store active team
All queries filter by team_id

```

## Prompt 13.6 — Invite Members
```

Add invite by email flow
Add invited users to team_members
Assign roles per invite

```

## Prompt 13.7 — RLS Updates
```

Update RLS policies:

* User must belong to team
* Role checks are team-scoped
* Prevent cross-team access

```

---

# PHASE 14 — SaaS / Future Enhancements

- Stripe billing
- Usage limits (issues/seats)
- Team settings page
- Multi-team audit logs

---

# MVP Completion Criteria

- Google login works
- Profiles auto-create
- Business users can submit tickets
- Developers can manage board
- RLS enforced

---

# Post-MVP Completion Criteria

- Users can create teams
- Users can join teams via invite
- Team data is isolated
- Roles are team-specific
- Queries respect team_id
```

