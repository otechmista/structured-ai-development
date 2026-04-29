# COMPONENT VIEW - TODO API
(Behavior, Rules, Validation, Internal Logic)

## Instructions for AI

- Do NOT generate full implementation code
- Focus on behavior and rules
- Do NOT invent business logic
- Expand only what is defined

---

## Scope

Which block is being detailed:

HTTP API, Task Service, Validator, Repository, Error Middleware, and Logger

---

## Components

| Component | Responsibility |
|---|---|
| TaskHandler | HTTP routing, request decoding, response writing, request operation logging |
| TaskService | Task use cases, validation orchestration, default values, partial updates |
| TaskRepository | SQLite persistence, ID generation, timestamp management, task retrieval and mutation |
| TaskValidator | Input validation and business rule enforcement |
| ErrorMiddleware | JSON error formatting and HTTP status mapping |
| Logger | Structured operation log lines |

---

## Component Specifications

### Component: TaskHandler

Responsibility: Route HTTP requests, parse input, return formatted responses, and log each operation

Inputs:
- HTTP requests: `POST /tasks`, `GET /tasks`, `GET /tasks/{id}`, `PUT /tasks/{id}`, `DELETE /tasks/{id}`
- Path parameter: task ID from `/tasks/{id}`
- Request body: JSON for create and update
- Optional `X-Request-ID` header

Outputs:
- HTTP responses: 200, 201, 204, 400, 404, 405, 500
- JSON response body for all successful responses except delete
- JSON error response body for validation, not found, and internal errors
- Operation log line per request

Business Rules:

- Route requests to the appropriate service method
- Parse JSON body safely for create/update
- Reject malformed JSON with validation error
- Reject nested paths under `/tasks/{id}/...` as invalid task IDs
- Return list responses wrapped as `{ "tasks": [...] }`
- Return `204 No Content` after a successful delete

Validations:

- Verify HTTP method matches supported endpoint
- Verify request body is valid JSON for POST and PUT
- Verify path does not contain additional segments after task ID
- Delegate field and ID validation to TaskService

Error Cases:

- Malformed JSON -> 400 Bad Request
- Missing required fields -> 400 Bad Request
- Invalid task ID path format -> 400 Bad Request
- Task not found -> 404 Not Found
- Unsupported method -> 405 Method Not Allowed
- Unexpected service error -> 500 Internal Server Error

Dependencies:

- TaskService interface
- ErrorMiddleware
- Logger

Does NOT do:

- Business field validation
- Direct repository access
- ID generation
- Timestamp management

---

### Component: TaskService

Responsibility: Implement task use cases and coordinate validation with persistence

Inputs:

- CreateTaskInput: title, description, optional priority, optional status
- UpdateTaskInput: optional title, description, priority, status
- Task ID for get, update, and delete

Outputs:

- Created task with ID and timestamps
- List of tasks
- Retrieved task
- Updated task
- Confirmation of deletion through nil error

Business Rules:

- Task title is required on create
- Priority defaults to `medium` if omitted on create
- Status defaults to `pending` if omitted on create
- Create accepts explicit valid status when provided
- Update is partial; only provided fields are changed
- Task ID is immutable after creation
- `createdAt` is preserved on update
- `updatedAt` changes on update

Validations:

- Create input is validated before defaulting and persistence
- Update ID is validated before lookup
- Update input is validated before lookup/update
- Delete and get IDs are validated before repository access

Error Cases:

- Empty or whitespace-only title -> Validation error
- Title with leading/trailing spaces -> Validation error
- Invalid priority -> Validation error
- Invalid status -> Validation error
- Missing task ID -> Validation error
- Task not found for get/update/delete -> Not found error

Dependencies:

- TaskRepository interface
- TaskValidator interface

Does NOT do:

- Direct HTTP handling
- JSON decoding or encoding
- Operation logging
- Low-level storage locking

---

### Component: TaskRepository

Responsibility: Persist and retrieve task data using SQLite

Inputs:

- Task object to create or update
- Task ID to retrieve/delete

Outputs:

- Saved task with ID and timestamps
- Retrieved task(s)
- Not found error when ID is absent
- Nil error on successful deletion

Business Rules:

- Assign unique UUID-style ID to new tasks
- Fall back to nanosecond timestamp string if random ID generation fails
- Store creation timestamp in UTC
- Store last update timestamp in UTC
- Preserve original creation timestamp on update
- Return tasks sorted by `createdAt` ascending
- Persist task data across process restarts

Validations:

- Verify task exists before update/delete/get
- Repository assumes service already validated business fields

Error Cases:

- Task ID not found -> `model.ErrTaskNotFound`

Dependencies:

- SQLite database access layer
- Go standard library for time and ID-generation helpers where applicable

Does NOT do:

- Business field validation
- HTTP handling
- JSON formatting
- Service orchestration

---

### Component: TaskValidator

Responsibility: Validate input data and business constraints

Inputs:

- CreateTaskInput
- UpdateTaskInput
- Task ID string

Outputs:

- Nil error when valid
- Error with a user-facing validation message when invalid

Business Rules:

- Enforce title, description, priority, status, and ID constraints
- Create requires title
- Update validates only fields that are present
- Priority and status are optional on create, but must be valid when present

Validations:

- Title: required when present, 1-255 characters after trimming, no leading/trailing spaces
- Description: optional, 0-1000 characters
- Priority: optional on create/update, must be `low`, `medium`, or `high` when present
- Status: optional on create/update, must be `pending` or `done` when present
- ID: must not be empty or whitespace-only

Error Cases:

- Title empty -> `title is required and must be 1-255 characters`
- Title has leading/trailing spaces -> `title must not have leading or trailing spaces`
- Priority invalid -> `priority must be one of: low, medium, high`
- Status invalid -> `status must be one of: pending, done`
- Description too long -> `description must be 0-1000 characters`
- ID empty -> `task id is required`

Dependencies:

- None beyond Go standard library and model types

Does NOT do:

- Modify data
- Persist data
- HTTP handling
- Check whether an ID exists in storage

---

## Internal Flow

Describe execution:

### Create Task Flow:
1. TaskHandler receives `POST /tasks`
2. TaskHandler decodes JSON into CreateTaskInput
3. TaskService validates create input through TaskValidator
4. If invalid: ErrorMiddleware returns 400
5. If valid: TaskService applies default priority/status when omitted
6. TaskService calls TaskRepository.Create
7. TaskRepository assigns ID, `createdAt`, and `updatedAt`
8. TaskRepository stores task in SQLite
9. TaskService returns created task
10. TaskHandler returns 201 Created with task JSON

### Get All Tasks Flow:
1. TaskHandler receives `GET /tasks`
2. TaskService calls TaskRepository.List
3. TaskRepository returns all tasks sorted by creation time
4. TaskService returns task list
5. TaskHandler returns 200 OK with `{ "tasks": [...] }`

### Update Task Flow:
1. TaskHandler receives `PUT /tasks/{id}`
2. TaskHandler extracts ID from path
3. TaskHandler decodes JSON into UpdateTaskInput
4. TaskService validates ID and update input
5. TaskService loads existing task from TaskRepository
6. If not found: ErrorMiddleware returns 404
7. If found: TaskService applies only provided fields
8. TaskRepository preserves `createdAt`, updates `updatedAt`, and stores the task
9. TaskHandler returns 200 OK with updated task

### Delete Task Flow:
1. TaskHandler receives `DELETE /tasks/{id}`
2. TaskHandler extracts ID from path
3. TaskService validates ID
4. TaskRepository verifies task exists
5. If not found: ErrorMiddleware returns 404
6. If found: TaskRepository deletes task
7. TaskHandler returns 204 No Content

---

## Edge Cases

List explicitly:

- Empty task list: Return `{ "tasks": [] }`, not an error
- Update non-existent task: Return 404 and do not create it
- Delete non-existent task: Return 404 and do not ignore silently
- Get non-existent task: Return 404
- Concurrent updates: SQLite transaction/locking behavior protects writes and last successful write wins
- Very long task list: No pagination for MVP
- Missing optional fields on update: Leave existing values unchanged
- Empty JSON update body: Valid no-op update that refreshes `updatedAt`
- Whitespace-only title: Reject
- Title with leading/trailing spaces: Reject
- Nested task path such as `/tasks/id/extra`: Return 400

---

## Data Transformations

Describe how data changes:

- Request JSON -> CreateTaskInput/UpdateTaskInput -> Validated input -> Task model -> Stored entity
- Stored entity -> Service result -> Response JSON
- Create applies defaults for omitted priority and status before persistence
- Update merges provided fields with the existing stored task
- Repository adds generated ID and timestamps
- Deletion removes the task record from SQLite completely

---

## Key Questions (AI must validate)

- Are responsibilities clear? Yes, each layer has one primary role
- Are rules complete? Yes, implemented create/update validation and defaults are explicit
- Are validations sufficient? Yes, title, description, priority, status, and ID are covered
- Are edge cases covered? Yes, empty lists, not found, malformed JSON, no-op updates, and concurrent updates are addressed
