# COMPONENT VIEW
(Behavior, Rules, Validation, Internal Logic)

## Instructions for AI

- Do NOT generate full implementation code
- Focus on behavior and rules
- Do NOT invent business logic
- Expand only what is defined

---

## Scope

Which block is being detailed:

SvelteKit App

---

## Components

| Component | Responsibility |
|---|---|
| TimelineRoute | Loads and renders home, following, and profile timelines |
| ThreadRoute | Loads a post, its ancestors, and replies |
| ComposeBox | Creates posts and replies |
| PostCard | Renders one post and available actions |
| ProfileHeader | Renders profile identity and follow state |
| NotificationList | Shows user notification events |
| ModerationReportDialog | Lets authenticated users report content |
| ApiClient | Performs backend requests and normalizes responses |
| SessionStore | Exposes current session state to interactive components |
| MutationStateStore | Tracks pending optimistic UI updates |

---

## Component Specifications

### Component: TimelineRoute

Responsibility:

Render a scrollable list of posts for home, following, or profile timelines.

Inputs:

- Route parameters
- Query parameters for pagination
- Session state
- Backend timeline response

Outputs:

- Timeline page
- Empty state
- Loading state
- Error state

Business Rules:

- Visitors may view public timelines.
- Authenticated-only timelines must redirect or show sign-in state when no session exists.
- Pagination must preserve existing posts while fetching the next page.

Validations:

- Timeline type must be supported.
- Cursor must be passed through unchanged when provided by the backend.

Error Cases:

- Backend unavailable
- Invalid timeline type
- Empty result

Dependencies:

- ApiClient
- PostCard
- SessionStore

Does NOT do:

- Rank posts locally
- Decide whether content is allowed

### Component: ComposeBox

Responsibility:

Allow authenticated users to create a post or reply.

Inputs:

- Draft text
- Optional media selection
- Optional parent post id for replies
- Session state

Outputs:

- Create mutation request
- Pending UI state
- Success callback
- Validation or system error

Business Rules:

- Only authenticated users may submit.
- Empty text and no media is invalid.
- Reply creation must include a parent id.
- Failed submissions must preserve the draft.

Validations:

- Text must not exceed configured character limit.
- Media count and media type must match backend constraints.
- Parent id is required for reply mode.

Error Cases:

- User is not authenticated
- Text is too long
- Media upload fails
- Backend rejects the mutation

Dependencies:

- ApiClient
- SessionStore
- MutationStateStore

Does NOT do:

- Persist posts locally as final state
- Bypass backend validation

### Component: PostCard

Responsibility:

Render a post, author metadata, content, reply count, like count, repost count, and action controls.

Inputs:

- Post entity
- Session state
- Pending mutation state

Outputs:

- Displayed post
- Navigation to thread or profile
- Like, repost, reply, and report actions

Business Rules:

- Deleted posts must display a deleted placeholder.
- Visitors may open posts and profiles but may not mutate.
- Like and repost buttons must reflect current backend state plus pending optimistic state.

Validations:

- Post id must exist.
- Action controls requiring auth must check session state before calling API.

Error Cases:

- Action rejected
- Post deleted between load and interaction
- Session expires during action

Dependencies:

- ApiClient
- ComposeBox for reply affordance
- ModerationReportDialog

Does NOT do:

- Render private content not returned by the API

### Component: ApiClient

Responsibility:

Centralize backend calls and normalize API errors for the UI.

Inputs:

- Endpoint path
- Request method
- Request body
- Session token or cookie context

Outputs:

- Typed data result
- Normalized error result

Business Rules:

- Mutating requests must include session credentials.
- API errors must be mapped into user-recoverable categories where possible.

Validations:

- Request body shape must match the frontend contract.
- Required parameters must be present before sending.

Error Cases:

- Network timeout
- Unauthorized
- Forbidden
- Rate limited
- Server error

Dependencies:

- Browser or server fetch
- Session source

Does NOT do:

- Invent fallback data
- Swallow errors silently

---

## Internal Flow

Describe execution:

1. Route loader fetches initial data through ApiClient.
2. Page renders route-level state.
3. Components receive typed data as props.
4. User interaction calls a component action.
5. Action validates local requirements.
6. MutationStateStore records pending state if optimistic UI is allowed.
7. ApiClient sends the backend request.
8. UI reconciles success or rolls back failure.

---

## Edge Cases

List explicitly:

- Timeline returns no posts.
- User signs out while composing.
- A post is deleted while the thread is open.
- A like is double-clicked before the first request completes.
- Pagination cursor expires.
- Media upload finishes after the user removes the media from draft.

---

## Data Transformations

Describe how data changes:

- backend post response -> Post entity -> PostCard props
- draft text and media selection -> create request -> backend post response -> inserted timeline item
- backend error -> normalized UI error -> route or component error state

---

## Key Questions (AI must validate)

- Are responsibilities clear?
- Are rules complete?
- Are validations sufficient?
- Are edge cases covered?
