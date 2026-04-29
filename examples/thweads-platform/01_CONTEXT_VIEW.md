# CONTEXT VIEW
(Problem, Business Intent, Boundaries)

## Instructions for AI

- Do NOT generate code
- Do NOT assume missing information
- Ask questions if context is unclear
- Focus on understanding the problem

---

## System Name

Thweads Platform

---

## Problem

People need a lightweight social posting experience that combines short public updates, threaded discussion, fast timeline browsing, and simple profile discovery without the complexity of a full social network.

The system must define the user-facing behavior for a SvelteKit frontend that can later be connected to a backend API.

---

## Users

- Human users:
  - Visitor browsing public posts
  - Authenticated user creating posts, replies, likes, and follows
  - Moderator reviewing reported content
- Systems:
  - SvelteKit frontend application
  - Backend API
  - Authentication provider
- External integrations:
  - Image hosting or media service
  - Analytics service

---

## Goals

What must be achieved:

- Provide a responsive social timeline interface.
- Allow users to view posts, threads, profiles, and notifications.
- Allow authenticated users to compose posts and replies.
- Keep frontend behavior clearly separated from backend implementation details.
- Support optimistic UI only where rollback behavior is explicit.

---

## Non-Goals

What is explicitly NOT part of the system:

- Building the backend API in this example.
- Implementing recommendation algorithms.
- Supporting paid subscriptions, ads, or monetization.
- Supporting direct messages.
- Supporting live audio or video.

---

## Business Rules (High Level)

Rules that affect behavior:

- Visitors may read public timelines, post detail pages, and public profiles.
- Only authenticated users may create posts, reply, like, repost, follow, or report content.
- A post may contain text and optional media.
- A reply belongs to exactly one parent post or reply.
- Deleted posts must not expose original text in the UI.
- Mutating actions must show success, loading, and failure states.
- Frontend must not invent moderation outcomes; it only displays backend decisions.

---

## Constraints

- Technical:
  - Frontend must use SvelteKit.
  - Application must support server-side rendering for public pages.
  - Client-side state must not be treated as the source of truth.
- Business:
  - UX should feel fast, focused, and familiar to users of short-post social apps.
  - The first version should prioritize timeline, posting, replies, and profiles.
- Legal:
  - Report and moderation UI must avoid exposing private reporter details.
  - User-generated content must be clearly attributed.
- Operational:
  - Network failures must be visible and recoverable.
  - Empty, loading, and error states must be designed for every primary view.

---

## System Boundaries

Inside the system:

- SvelteKit routes and layouts
- Timeline, thread, profile, compose, notification, and moderation UI
- Frontend data loading and mutation calls
- Frontend validation and user feedback states

Outside the system:

- Backend persistence
- Authentication token issuance
- Recommendation ranking
- Moderation decision engine
- Media processing pipeline

---

## Key Questions (AI must validate)

- Is the problem clear?
- Are goals measurable?
- Are constraints realistic?
- Are there missing actors?
