# CODE VIEW
(Implementation, Structure, Contracts)

## Instructions for AI

- Generate code ONLY based on previous views
- Do NOT invent new business rules
- Do NOT change architecture
- Follow structure strictly
- Keep code simple

---

## Language

TypeScript with SvelteKit

---

## Project Structure

Describe the folder and file organization:

```txt
src/
+-- lib/
|   +-- api/
|   |   +-- client.ts
|   |   +-- errors.ts
|   |   +-- types.ts
|   +-- components/
|   |   +-- ComposeBox.svelte
|   |   +-- ModerationReportDialog.svelte
|   |   +-- NotificationList.svelte
|   |   +-- PostCard.svelte
|   |   +-- ProfileHeader.svelte
|   |   +-- TimelineList.svelte
|   +-- stores/
|   |   +-- mutation-state.ts
|   |   +-- session.ts
|   +-- validation/
|       +-- compose.ts
+-- routes/
|   +-- +layout.svelte
|   +-- +page.server.ts
|   +-- +page.svelte
|   +-- post/
|   |   +-- [id]/
|   |       +-- +page.server.ts
|   |       +-- +page.svelte
|   +-- profile/
|   |   +-- [handle]/
|   |       +-- +page.server.ts
|   |       +-- +page.svelte
|   +-- notifications/
|       +-- +page.server.ts
|       +-- +page.svelte
tests/
+-- unit/
+-- integration/
```

---

## Entities

Define core entities:

| Entity | Description |
|---|---|
| User | Public profile identity, handle, display name, avatar, bio, and follow state |
| Post | Short text/media item with author, counts, visibility, and lifecycle state |
| Thread | A post with ancestor and reply relationships |
| MediaAttachment | Image or media metadata attached to a post |
| Notification | User-facing event such as reply, like, repost, follow, or moderation update |
| Session | Current authentication and permission state |
| ApiError | Normalized error returned by ApiClient |

---

## API Contracts

List endpoints:

| Method | Path | Description |
|---|---|---|
| GET | /api/timeline/public | Fetch public timeline |
| GET | /api/timeline/following | Fetch authenticated following timeline |
| GET | /api/posts/:id | Fetch post detail and thread context |
| POST | /api/posts | Create a root post |
| POST | /api/posts/:id/replies | Create a reply |
| POST | /api/posts/:id/likes | Like a post |
| DELETE | /api/posts/:id/likes | Remove like from a post |
| POST | /api/posts/:id/reposts | Repost a post |
| DELETE | /api/posts/:id/reposts | Remove repost |
| GET | /api/users/:handle | Fetch public profile |
| POST | /api/users/:id/follows | Follow a user |
| DELETE | /api/users/:id/follows | Unfollow a user |
| GET | /api/notifications | Fetch authenticated notifications |
| POST | /api/reports | Report a post |

---

## Implementation Rules

Enforce architecture:

- Keep route loaders responsible for route data only.
- Keep reusable rendering in Svelte components.
- Keep backend calls inside ApiClient.
- Keep compose validation in `lib/validation`.
- Keep optimistic state explicit and reversible.
- No backend persistence logic in the SvelteKit frontend.
- No hidden mutation inside display-only components.

---

## Error Handling

Define error categories:

| Error Type | Description | Handling |
|---|---|---|
| Validation errors | Draft, route parameter, or action input is invalid | Show inline error and prevent request |
| Business errors | Backend rejects action due to auth, permissions, deletion, or rate limit | Roll back pending state and show recoverable message |
| System errors | Network timeout, unavailable backend, unexpected server response | Show route or action error with retry |

---

## Logging

Must include:

- request_id
- operation
- error
- duration

Frontend logging must avoid logging post draft text, auth tokens, or private reporter details.

---

## Tests

Required:

- Business rules
- Edge cases
- Failure scenarios

Specific tests:

- ComposeBox blocks empty submissions.
- ComposeBox preserves draft after failed request.
- PostCard redirects unauthenticated mutation attempts to sign-in behavior.
- TimelineRoute renders empty, loading, and error states.
- Optimistic like state rolls back on rejection.
- ThreadRoute handles deleted parent posts.
- ApiClient normalizes unauthorized, forbidden, rate limit, and network errors.

---

## Performance & Security Constraints

- Performance:
  - SSR public pages.
  - Paginate timelines.
  - Avoid refetching unchanged profile metadata unnecessarily.
- Scalability:
  - Keep frontend state bounded to visible data and active drafts.
  - Defer heavy ranking and search to backend services.
- Security:
  - Never expose auth tokens to logs.
  - Do not trust frontend validation as final authorization.
  - Escape and render user-generated content safely.

---

## Key Questions (AI must validate)

- Does code match architecture?
- Are rules implemented?
- Are edge cases handled?
- Is complexity justified?
