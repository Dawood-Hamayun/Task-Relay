# TaskRelay — Real-Time Project Workspace

TaskRelay is a full-stack, real-time project workspace for teams. It combines tasks, subtasks, meetings, scheduling, commenting, tagging, and role-based permissions into a single collaborative environment. The project demonstrates how modern SaaS collaboration tools are architected end-to-end with real-time updates, structured domain models, and predictable permission rules.

The focus of TaskRelay is not visual polish or marketing—it’s about how collaboration actually works at the technical and product level.

---

## Core Idea

Most task apps are designed for individuals. Team collaboration requires more than tasks:

- permissions
- invites
- meetings
- scheduling
- comments
- tagging
- real-time sync
- onboarding flows

TaskRelay implements those pieces and shows how they interact in a cohesive system.

---

## User-Level Capabilities

Users can:

- create projects
- invite team members by email
- assign roles
- create tasks and subtasks
- attach files
- comment and discuss
- apply tags
- schedule meetings
- RSVP to events
- drag-and-drop tasks on a kanban board
- view task and personal stats

A typical workflow looks like:

1. Create a project
2. Invite collaborators
3. Assign roles
4. Add tasks → comments → tags
5. Schedule meetings
6. Collaborate in real time

---

## Data Model Overview

The platform uses a normalized relational model with the following core entities:

- User
- Project
- Member (join model + roles)
- Task
- Subtask
- Comment
- Tag
- Meeting
- MeetingAttendee
- Invite
- Attachment

High-level structure:

```
User (many) ←→ (many) Project via Member
Project → Tasks → Subtasks / Comments / Attachments
Project → Meetings → Attendees
Tasks ←→ Tags (many-to-many)
Invites operate before signup (email first, user later)
```

This mirrors real SaaS collaboration patterns without collapsing the data into blobs.

---

## Backend Architecture

Backend stack:

- NestJS (TypeScript)
- PostgreSQL
- Prisma ORM
- Socket.io (WebSockets)
- JWT auth
- Nodemailer (email invites)

Server responsibilities:

- domain logic (projects, tasks, meetings, invites)
- access control checks
- email onboarding
- real-time broadcasting
- state transitions
- validation and error handling

The backend acts as the source of truth. Clients optimistically update and reconcile.

---

## Frontend Architecture

Frontend stack:

- React
- Next.js App Router
- TanStack Query (server state)
- Socket.io client
- shadcn/ui
- DnD Kit (drag-and-drop)

Client responsibilities:

- rendering workspace
- caching + invalidations
- optimistic UI
- socket subscriptions
- visual organization of tasks + meetings

Server data is cached and merged with real-time events.

---

## Real-Time Collaboration Model

TaskRelay uses WebSockets to synchronize state across users:

Events:

- `taskCreated`
- `taskUpdated`
- `taskDeleted`

Mechanics:

- clients join project rooms
- server emits to only those rooms
- views update instantly
- no polling required

Optimistic updates reduce drag latency on the Kanban board.

---

## Roles & Permissions

The system implements RBAC with four roles:

- **Owner** — full control + transfer ownership
- **Admin** — manage members/content
- **Member** — standard collaborator
- **Viewer** — read-only

Permissions enforced on backend, not just hidden in UI. This ensures correctness in multi-user flows like:

- deleting a project
- updating roles
- removing members
- managing invites
- mutation rights on tasks/meetings/comments

---

## Invitation & Onboarding Flow

Invitations are email-based and work prior to signup:

1. Invite is sent to email
2. Invite stored with token + expiry
3. Email link allows accept/decline
4. Non-users can sign up and auto-join
5. On acceptance → `Member` record created

This matches how tools like Asana, Linear, and Notion onboard teams.

---

## State Management Strategy

TaskRelay uses TanStack Query for server state:

- cached per project
- stale-while-revalidate
- optimistic updates
- refetch on focus
- granular invalidation

This reduces unnecessary fetches and keeps UI in sync with WebSockets.

---

## Meetings & Scheduling Domain

Projects support integrated scheduling including:

- title
- description
- datetime
- attendees
- RSVP statuses (pending, accepted, declined)
- time range queries (week, upcoming, user-level)

This demonstrates that collaboration involves more than tasks—it involves time and coordination.

---

## Engineering Notes

TaskRelay demonstrates:

- relational data modeling
- real-time event systems
- access control and permissions
- email-based onboarding
- optimistic UI patterns
- normalized state management
- drag-and-drop interaction
- server/client coordination
- meeting + scheduling domain logic

---

## What This Project Is (and Is Not)

TaskRelay is:

- a functioning demonstration of collaborative SaaS architecture
- real-time and multi-user
- based on TypeScript end-to-end
- designed with domain correctness in mind

TaskRelay is not:

- a design exercise
- a clone for commercial use
- optimized for scale or billing
- open-source (code is not included here)

---

## Status

Feature-complete as a demonstration of:

- real-time collaboration
- normalized relational domains
- role-based access control
- email onboarding
- workspace UX

Source code not included in this repository.

