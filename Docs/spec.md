# Backlogr — Product Specification

Version: 1.0  
Status: MVP + Post-MVP Multi-Tenant Expansion Defined  
Stack: Next.js 14 (App Router) + Typescript + Supabase + Tailwind  
Auth: Google OAuth (Supabase Auth)

---

# 1. Product Overview

Backlogr is a lightweight internal help desk that bridges business requests and developer workflows.

Non-technical users submit issues through a clean intake interface.
Developers manage, prioritize, and execute work through a structured Kanban board.

Clear intake. Structured execution. No noise.

---

# 2. Goals

## MVP Goals

- Allow business users to submit tickets
- Allow developers to manage tickets via Kanban board
- Enforce role-based access control
- Maintain clean separation between Business UI and Developer UI
- Keep architecture simple and extensible

## Post-MVP Goals

- Support multiple teams (multi-tenant SaaS)
- Isolate team data
- Allow team-level role management
- Prepare system for billing & subscriptions

---

# 3. User Roles (MVP)

Roles are stored in `profiles.role`.

## Business
- Can create issues
- Can view their own issues
- Cannot change status
- Cannot assign issues

## Developer
- Can view all issues
- Can update status
- Can assign issues
- Can edit issue details

## Admin
- Can manage user roles
- Has all developer permissions

---

# 4. Core Features (MVP)

## 4.1 Authentication

- Google OAuth via Supabase
- Automatic profile creation on first login
- Default role: `business`

---

## 4.2 Business Interface (`/submit`)

Features:
- Ticket submission form:
  - Title (required)
  - Description (required)
  - Priority (low | medium | high)
- View previously submitted tickets
- See ticket status
- No Kanban visibility
- No technical controls

Design Philosophy:
Simple. Clean. Non-technical.

---

## 4.3 Developer Interface (`/board`)

Features:
- 4 Kanban columns:
  - Backlog
  - Todo
  - In Progress
  - Done
- Drag & drop status changes
- Issue detail modal
- Assign developer
- Edit priority
- View creator

Design Philosophy:
Execution-focused. Minimal distractions.

---

# 5. Data Model (MVP)

## 5.1 profiles

id (uuid, references auth.users)  
email (text)  
full_name (text)  
role (enum: business | developer | admin)  
created_at (timestamp)

---

## 5.2 issues

id (uuid)  
title (text)  
description (text)  
status (enum: backlog | todo | in_progress | done)  
priority (enum: low | medium | high)  
created_by (uuid → profiles.id)  
assigned_to (uuid → profiles.id, nullable)  
created_at (timestamp)  
updated_at (timestamp)

Defaults:
- status = backlog
- priority = medium

---

# 6. Security Model (MVP)

Row Level Security (RLS) enforced in Supabase.

## Profiles
- Users can read their own profile
- Admin can update roles

## Issues

Business:
- Can insert issues
- Can read issues where created_by = auth.uid()
- Cannot update status

Developers:
- Can read all issues
- Can update status
- Can assign issues

All enforcement must happen at database level.

---

# 7. Routing Logic

On login:

If role = business → redirect to `/submit`  
If role = developer → redirect to `/board`  
If role = admin → redirect to `/board`

Unauthorized routes must be blocked.

---

# 8. Real-Time Updates (Optional)

- Supabase realtime subscription on issues
- Board updates on INSERT and UPDATE

---

# 9. Non-Goals (MVP)

- No billing
- No multi-tenancy
- No public signup
- No comments
- No attachments
- No SLA tracking

---

# 10. Post-MVP — Multi-Tenant Expansion

This converts Backlogr from internal tool to SaaS platform.

---

## 10.1 New Core Concept

Data becomes scoped by team.

Users belong to teams.
Issues belong to teams.
Roles become team-specific.

---

## 10.2 New Tables

### teams

id (uuid)  
name (text)  
created_by (uuid → profiles.id)  
created_at (timestamp)

---

### team_members

id (uuid)  
team_id (uuid → teams.id)  
user_id (uuid → profiles.id)  
role (enum: business | developer | admin)  
created_at (timestamp)

Users can belong to multiple teams.

---

## 10.3 Updated issues Table

Add:

team_id (uuid → teams.id, NOT NULL)

All queries must filter by team_id.

---

# 11. Multi-Tenant Authorization Model

Access requires:

1. User is authenticated
2. User is member of the team
3. Role permissions apply within that team

Roles are no longer global.
They are scoped per team.

Profiles become identity only.
team_members defines authorization.

---

# 12. Team Creation Flow

Authenticated user can:

- Create new team
- Become admin of that team
- Invite users via email

Users without a team:
- Redirected to onboarding flow

---

# 13. Data Isolation Rules

- No cross-team visibility
- No cross-team issue access
- No cross-team assignment
- All RLS policies must enforce team membership

---

# 14. SaaS-Ready Future Enhancements

- Stripe billing
- Team-level subscription tiers
- Seat limits
- Usage limits
- Audit logs
- Team settings page
- Invite management dashboard

---

# 15. Architectural Principles

- Keep schema minimal
- Enforce security at database level
- Separate Business and Developer mental models
- Design for migration to multi-tenancy
- Avoid premature complexity

---

# 16. MVP Completion Criteria

Backlogr MVP is complete when:

- Google authentication works
- Profiles auto-create
- Business users can submit tickets
- Developers can manage board
- RLS prevents unauthorized access
- Status updates persist correctly

---

END OF SPEC
