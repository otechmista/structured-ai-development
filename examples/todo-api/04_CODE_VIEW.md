# CODE VIEW - TODO API
(Implementation, Structure, Contracts)

## Instructions for AI

- Generate code ONLY based on previous views
- Do NOT invent new business rules
- Do NOT change architecture
- Follow structure strictly
- Keep code simple

---

## Language

Go (Golang)

---

## Go Module and Execution

Required:

- The API source code lives in `example/todo-api/src`.
- `example/todo-api/src/go.mod` declares `module todo-api`.
- The workspace file lives at `example/go.work` and includes:

```txt
go 1.22

use ./todo-api/src
```

- Run the API as a package, not as a standalone file:

```bash
cd example/todo-api/src
go run ./cmd/server
```

- If using VS Code Code Runner, configure Go to run from the package directory with `go run .` or run `go run ./cmd/server` from the module root.

Reason:

- Go internal imports such as `todo-api/internal/handler` are resolved through the module/workspace. Running `cmd/server/main.go` directly from outside the module can make Go search the standard library path and fail with `package todo-api/internal/... is not in std`.

---

## Project Structure

Describe the folder and file organization:

```txt
todo-api/
+-- docs/
|   +-- 01_CONTEXT_VIEW.md
|   +-- 02_CONTAINER_VIEW.md
|   +-- 03_COMPONENT_VIEW.md
|   +-- 04_CODE_VIEW.md
+-- src/
    +-- cmd/
    |   +-- server/
    |       +-- main.go                 # Application entry point
    +-- internal/
    |   +-- handler/
    |   |   +-- task.go                 # HTTP endpoint handlers and request logging wrapper
    |   +-- service/
    |   |   +-- errors.go               # Service-level error types and helpers
    |   |   +-- task.go                 # Business logic and interfaces
    |   +-- repository/
    |   |   +-- task.go                 # SQLite data persistence
    |   +-- validator/
    |   |   +-- task.go                 # Input validation
    |   +-- model/
    |   |   +-- errors.go               # Domain errors
    |   |   +-- task.go                 # Task struct, input structs, enums, list response
    |   +-- middleware/
    |       +-- error.go                # JSON and error response helpers
    +-- pkg/
    |   +-- logger/
    |       +-- logger.go               # Logging utility
    +-- test/
    |   +-- service_test.go
    |   +-- validator_test.go
    |   +-- integration_test.go
    +-- go.mod
    +-- request.http                    # Manual HTTP requests for local testing
```

---

## Entities

Define core entities:

| Entity | Description |
|---|---|
| Task | Core domain entity with id, title, description, priority, status, createdAt, updatedAt |
| CreateTaskInput | Input for creating a task; title required, description/priority/status optional |
| UpdateTaskInput | Input for partially updating a task; all fields optional pointers |
| TaskListResponse | Response wrapper for list endpoint: `{ "tasks": [...] }` |
| Priority | Enum: low, medium, high |
| Status | Enum: pending, done |

---

## API Contracts

List endpoints:

| Method | Path | Description |
|---|---|---|
| POST | /tasks | Create new task |
| GET | /tasks | List all tasks |
| GET | /tasks/{id} | Get single task |
| PUT | /tasks/{id} | Partially update existing task |
| DELETE | /tasks/{id} | Delete task |

### Core Types (Go)

```go
type Task struct {
	ID          string    `json:"id"`
	Title       string    `json:"title"`
	Description string    `json:"description,omitempty"`
	Priority    Priority  `json:"priority"`
	Status      Status    `json:"status"`
	CreatedAt   time.Time `json:"createdAt"`
	UpdatedAt   time.Time `json:"updatedAt"`
}

type CreateTaskInput struct {
	Title       string   `json:"title"`
	Description string   `json:"description,omitempty"`
	Priority    Priority `json:"priority,omitempty"`
	Status      Status   `json:"status,omitempty"`
}

type UpdateTaskInput struct {
	Title       *string   `json:"title,omitempty"`
	Description *string   `json:"description,omitempty"`
	Priority    *Priority `json:"priority,omitempty"`
	Status      *Status   `json:"status,omitempty"`
}

type TaskListResponse struct {
	Tasks []Task `json:"tasks"`
}

type Priority string

const (
	PriorityLow    Priority = "low"
	PriorityMedium Priority = "medium"
	PriorityHigh   Priority = "high"
)

type Status string

const (
	StatusPending Status = "pending"
	StatusDone    Status = "done"
)
```

### Request/Response Examples

**POST /tasks**
```json
Request Body:
{
  "title": "Buy groceries",
  "description": "Milk, eggs, bread",
  "priority": "high"
}

Response 201:
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "title": "Buy groceries",
  "description": "Milk, eggs, bread",
  "priority": "high",
  "status": "pending",
  "createdAt": "2026-04-29T10:30:00Z",
  "updatedAt": "2026-04-29T10:30:00Z"
}
```

**GET /tasks**
```json
Response 200:
{
  "tasks": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "title": "Buy groceries",
      "description": "Milk, eggs, bread",
      "priority": "high",
      "status": "pending",
      "createdAt": "2026-04-29T10:30:00Z",
      "updatedAt": "2026-04-29T10:30:00Z"
    }
  ]
}
```

**PUT /tasks/{id}**
```json
Request Body:
{
  "status": "done"
}

Response 200:
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "title": "Buy groceries",
  "description": "Milk, eggs, bread",
  "priority": "high",
  "status": "done",
  "createdAt": "2026-04-29T10:30:00Z",
  "updatedAt": "2026-04-29T10:35:00Z"
}
```

**DELETE /tasks/{id}**
```txt
Response 204 with no body
```

---

## Implementation Rules

Enforce architecture:

- Keep layers separated: Handler -> Service -> Repository
- Use Go standard library HTTP routing with `http.ServeMux`
- No business field validation in handlers; delegate to service/validator
- No persistence logic outside repository
- No hidden side effects beyond repository-managed ID/timestamps and logging
- Timestamps managed by repository only
- ID generation handled by repository only
- Use interfaces for dependency injection between handler/service/repository/validator
- Error handling uses Go's `error` type and sentinel/domain errors
- Use SQLite for durable local persistence through the repository layer
- Receiver methods must be pointer receivers for handlers/services/repositories/validators where state or interface consistency matters
- All package names lowercase, no underscores or CamelCase

---

## Error Handling

Define error categories:

| Error Type | HTTP Status | Description | Example |
|---|---|---|---|
| Validation errors | 400 | Input validation failed | Empty title, malformed JSON, empty ID |
| Not Found errors | 404 | Task ID does not exist | GET /tasks/missing |
| Method Not Allowed | 405 | Route exists but HTTP method is unsupported | PATCH /tasks/id |
| Internal Server Error | 500 | Unexpected server/service error | Storage failure |

Error Response Format:
```json
{
  "error": "Validation Error",
  "message": "title is required and must be 1-255 characters",
  "code": "VALIDATION_ERROR",
  "timestamp": "2026-04-29T10:30:00Z"
}
```

Error code mapping:

| Code | Trigger |
|---|---|
| VALIDATION_ERROR | Service validation errors and malformed JSON |
| NOT_FOUND | `model.ErrTaskNotFound` / `service.ErrNotFound` |
| INTERNAL_SERVER_ERROR | Unexpected errors |

---

## Logging

Must include:

- request_id: `X-Request-ID` header value, or generated nanosecond timestamp when header is absent
- operation: Name of operation (`CREATE_TASK`, `GET_TASKS`, `GET_TASK`, `UPDATE_TASK`, `DELETE_TASK`, or `UNKNOWN_OPERATION`)
- status: `success` for HTTP status below 400, otherwise `error`
- error: Error message if operation failed
- duration: Time taken in milliseconds

Log Format:
```txt
[2026-04-29T10:30:00Z] request_id=abc123 operation=CREATE_TASK status=success duration=45ms
[2026-04-29T10:30:05Z] request_id=abc124 operation=UPDATE_TASK status=error error="http status 404" duration=12ms
```

---

## Tests

Required:

**Business Rules Tests:**
- TestTaskDefaultsToPendingStatus: New task defaults to pending status
- TestPriorityDefaultsToMedium: Priority defaults to medium if not provided
- TestTitleRequired: Task title is required
- TestValidPriorities: Only valid priorities accepted (low, medium, high)

**Edge Cases Tests:**
- TestEmptyTaskListReturnsEmptyArray: Empty task list returns an empty slice and serializes as an empty task list
- TestUpdateNonExistentTaskReturns404: Updating non-existent task returns 404
- TestDeleteNonExistentTaskReturns404: Deleting non-existent task returns 404
- TestConcurrentUpdatesHandled: Concurrent updates are serialized by the repository
- TestWhitespaceOnlyTitleRejected: Whitespace-only title is rejected

**Failure Scenario Tests:**
- TestInvalidJSONBodyReturns400: Invalid JSON body returns 400
- TestMissingRequiredFieldsReturns400: Missing required fields returns 400
- TestInvalidTaskIDFormatReturns400: Invalid task ID path format returns 400
- TestTaskNotFoundReturns404: Task not found returns 404
- TestStorageFailureReturns500: Unexpected service/storage failure returns 500

**Test Convention:**
- Tests use Go's standard testing package (`testing.T`)
- Table-driven tests are preferred for multiple scenarios
- Integration tests use `net/http/httptest`
- Tests live in `example/todo-api/src/test`

**Manual HTTP Tests:**
- Include `request.http` at the API module root (`example/todo-api/src/request.http`)
- Use `@baseUrl = http://localhost:8080`
- Include requests for:
  - `GET /tasks`
  - `POST /tasks`
  - `GET /tasks/{id}`
  - `PUT /tasks/{id}`
  - `DELETE /tasks/{id}`
  - Validation error scenarios
  - Not found scenario
- The API must be running before executing requests:

```bash
cd example/todo-api/src
go run ./cmd/server
```

---

## Performance & Security Constraints

- Performance: Response time should be low for typical SQLite-backed local operations
- Scalability: No session state, but SQLite is local to one running deployment unless explicitly shared at the infrastructure level
- Security: No authentication required for MVP; validation rejects invalid task input
- Go-specific: Use `ReadHeaderTimeout` on the HTTP server
- Storage: Tasks stored in SQLite with no separate caching layer
- Concurrency: Use SQLite transaction behavior for repository consistency
- Graceful shutdown: Handle SIGINT and SIGTERM with `http.Server.Shutdown`

---

## Key Questions (AI must validate)

- Does code match architecture? Yes, Handler-Service-Repository layers are implemented
- Are rules implemented? Yes, defaults, validation, not found handling, and SQLite persistence match the component view
- Are edge cases handled? Yes, not found, validation, invalid JSON, empty lists, and concurrent updates are covered
- Is complexity justified? Yes, the implementation is small and uses only the Go standard library
