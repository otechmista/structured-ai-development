# CONTAINER VIEW
(System Structure, Architecture, Data Flow)

## Instructions for AI

- Do NOT generate code
- Do NOT change business rules
- Respect constraints from CONTEXT_VIEW
- Focus on system structure

---

## Architecture Style

Select one:

- hybrid

Justify the choice.

Thweads Platform is a SvelteKit frontend with SSR routes, browser-side interactions, and API integration. The frontend is not a full monolith because persistence, authentication, media processing, and moderation decisions live outside this example. It is hybrid at the frontend level because public pages use server-side loading while interactive actions run in the browser.

---

## System Blocks

List all major blocks:

| Block | Responsibility | Technology (optional) |
|---|---|---|
| SvelteKit App | Owns routes, layouts, page rendering, and frontend composition | SvelteKit |
| Route Loaders | Fetch page data for timelines, profiles, threads, and notifications | SvelteKit load functions |
| UI Components | Render reusable social UI elements | Svelte components |
| Client Stores | Hold short-lived UI state such as compose drafts and optimistic mutations | Svelte stores |
| API Client | Encapsulates calls to backend endpoints | fetch wrapper |
| Backend API | Provides posts, users, follows, likes, reports, and notifications | External |
| Auth Provider | Provides authenticated session state | External |
| Media Service | Stores and serves uploaded media | External |

---

## Data Flow

Describe step-by-step:

1. User opens a public route such as home, profile, or thread.
2. SvelteKit route loader requests data from the backend API.
3. The page renders SSR HTML for initial content.
4. Browser hydrates interactive components.
5. User performs an action such as post, reply, like, repost, follow, or report.
6. API client sends the mutation with the current session token.
7. UI displays loading state and may apply an explicit optimistic update.
8. On success, UI reconciles with backend response.
9. On failure, UI rolls back optimistic state and shows a recoverable error.

---

## External Integrations

| System | Purpose | Protocol |
|---|---|---|
| Backend API | Read and mutate social data | HTTPS JSON |
| Auth Provider | Resolve session and auth state | HTTPS/OAuth or cookie session |
| Media Service | Upload and retrieve post media | HTTPS |
| Analytics Service | Track product usage events | HTTPS |

---

## Failure Scenarios

| Failure | Impact | Handling Strategy |
|---|---|---|
| Backend API unavailable | Timelines and mutations cannot load | Show route error state and retry action |
| Session expired | Mutating actions fail | Prompt user to sign in again |
| Media upload fails | Post cannot include selected media | Keep draft text and show upload error |
| Optimistic mutation rejected | UI may show incorrect temporary state | Roll back state and display error |
| Rate limit reached | User cannot continue rapid actions | Display rate limit message and cooldown |

---

## Trade-offs

List architectural decisions:

- SSR is used for public readability and shareable pages.
- Client stores are limited to UI state and must not replace backend state.
- Optimistic UI is allowed only for low-risk actions with rollback behavior.
- Frontend does not perform final authorization decisions.

---

## Constraints Applied

The architecture keeps backend implementation outside the example, uses SvelteKit for SSR and interaction, and separates reusable UI components from route-level data fetching.

---

## Key Questions (AI must validate)

- Are responsibilities well separated?
- Is the architecture over-engineered?
- Are failure points addressed?
- Is there a simpler alternative?
