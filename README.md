# TaskRelay — Real-Time Collaborative Workspace

TaskRelay is a full-stack, real-time collaborative workspace for teams. It brings together tasks, subtasks, meetings, scheduling, commenting, tagging, and role-based permissions in a single environment. The project focuses on how modern SaaS collaboration tools work under the hood rather than just UI polish.

<p align="center">
  <img src="./assets/TaskRelay.gif" width="800">
  <br/>
  <em>Demo — Task Relay</em>
</p>

---

## What Problem It Explores

Most task apps are designed for individuals. Team collaboration has additional requirements:

- shared workspaces
- roles and permissions
- invitations
- meetings and scheduling
- comments and discussion
- tagging and filtering
- real-time synchronization
- onboarding and access control

TaskRelay implements these concerns and shows how they interact in practice.

---

## Feature Overview (User-Level)

Users can:

- create projects
- invite collaborators by email
- assign roles
- create tasks and subtasks
- attach files
- discuss via comments
- apply tags
- schedule meetings
- RSVP to events
- drag tasks on a Kanban board
- view personal and project statistics

A typical workflow:

1. Create a workspace
2. Invite collaborators
3. Assign project roles
4. Add tasks → comments → tags
5. Schedule meetings
6. Work together in real time

---

## Data Model (High-Level)

The platform uses a normalized relational schema with these core entities:

- User
- Project
- Member (join + role)
- Task
- Subtask
- Comment
- Tag
- Meeting
- MeetingAttendee
- Invite
- Attachment

Relationship sketch:

```
User ←→ Project via Member
Project → Tasks → Subtasks / Comments / Attachments
Project → Meetings → Attendees
Tasks ←→ Tags (many-to-many)
Invites operate before signup (email first, user later)
```

This mirrors patterns seen in SaaS work tools like Linear, Asana, Notion, and ClickUp.

---

## Backend Architecture

Backend stack:

- NestJS (TypeScript)
- PostgreSQL
- Prisma ORM
- Socket.io (WebSockets)
- JWT authentication
- Nodemailer (email invites)

Backend responsibilities:

- domain logic (tasks, projects, meetings, invites)
- access control
- email onboarding
- real-time broadcasting
- input validation
- error handling

The server acts as the source of truth. Clients reconcile through optimistic updates and real-time events.

---

## Frontend Architecture

Frontend stack:

- React
- Next.js (App Router)
- TanStack Query (server state)
- Socket.io client
- shadcn/ui (component primitives)
- DnD Kit (drag-and-drop interactions)

Client responsibilities:

- workspace rendering
- caching and invalidation
- optimistic updates
- socket subscription
- merging real-time updates into local state

---

## Real-Time Collaboration

TaskRelay uses WebSockets to keep multiple clients in sync.

Events include:

- `taskCreated`
- `taskUpdated`
- `taskDeleted`

Mechanics:

- clients join project-specific rooms
- backend emits only to those rooms
- Kanban board updates instantly
- no polling required
- optimistic drag-and-drop reduces perceived latency

---

## Roles & Access Control

The system defines four project roles:

- **Owner** — full control, can transfer ownership
- **Admin** — manage members and content
- **Member** — can collaborate on tasks and meetings
- **Viewer** — read-only

Access rules are enforced on the backend, not just the UI. This prevents invalid mutations during collaborative workflows (e.g., removing a member, editing meetings, updating tasks).

---

## Invitations & Onboarding

Invitations are email-based and operate before signup:

1. Owner/Admin sends invite to email
2. Invite stored with token + expiry
3. Recipient can accept/decline
4. Non-users can sign up and auto-join
5. Accepted invites create a `Member` record

This is the onboarding pattern used in production collaboration tools.

---

## State Management Strategy

TaskRelay uses TanStack Query for server state:

- cached per project
- stale-while-revalidate
- optimistic mutations
- refetch on window focus
- granular invalidation

Real-time events merge into the local cache instead of force-refreshing entire views.

---

## Meetings & Scheduling

Projects support integrated scheduling:

- title
- description
- datetime
- attendees
- RSVP statuses

Features include:

- upcoming views
- weekly and user-scoped queries
- RSVP management
- meeting metadata

This models coordination beyond tasks.

---

## What This Project Demonstrates

TaskRelay highlights:

- relational data modeling
- domain-driven collaboration features
- access control and permissions
- real-time event systems
- email-based onboarding flows
- optimistic UI patterns
- workspace UX design
- drag-and-drop interaction
- server/client coordination

---

## Scope & Status

TaskRelay is:

- functional as a collaborative workspace demo
- based on TypeScript end-to-end
- real-time and multi-user aware
- architected for correctness, not just aesthetics

It is not:

- a commercial product
- a design showcase
- optimized for billing, teams, or scale
- open-source (source code not provided here)

