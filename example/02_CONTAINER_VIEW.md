# CONTAINER VIEW - TODO API
(System Structure, Architecture, Data Flow)

## Instructions for AI

- Do NOT generate code
- Do NOT change business rules
- Respect constraints from CONTEXT_VIEW
- Focus on system structure

---

## Architecture Style

Select one:

- monolith yes

Justify the choice:

The TODO API is a single Go process with one HTTP server, one repository layer, and a local SQLite database. A monolithic architecture keeps the system easy to run, test, and reason about for the current scope while still providing durable task storage.

---

## System Blocks

List all major blocks:

| Block | Responsibility | Technology (optional) |
|---|---|---|
| HTTP Server | Accept requests, expose routes, handle graceful shutdown | Go `net/http` |
| Task Handler | Route HTTP methods, decode JSON, encode responses, apply request logging | `internal/handler` |
| Task Service | Apply task use cases and defaults, coordinate validation and repository operations | `internal/service` |
| Task Repository | Store, retrieve, update, delete, sort, and timestamp tasks | `internal/repository` |
| Database | Durable local task persistence | SQLite |
| Task Validator | Validate title, description, priority, status, and non-empty IDs | `internal/validator` |
| Error Middleware | Convert domain/service errors to JSON HTTP responses | `internal/middleware` |
| Logger | Write operation logs with request ID, operation, status, error, and duration | `pkg/logger` |

---

## Data Flow

Describe step-by-step:

1. Client sends an HTTP request to `/tasks` or `/tasks/{id}`
2. TaskHandler matches the HTTP method and path
3. TaskHandler decodes JSON for create/update requests
4. TaskService validates IDs and task input through TaskValidator
5. TaskService applies defaults for omitted priority/status on create
6. TaskRepository reads or mutates task records in SQLite
7. TaskRepository assigns IDs and timestamps for creates, and updates `updatedAt` for updates
8. TaskHandler returns JSON success responses or delegates errors to Error Middleware
9. Logger records operation name, request ID, status, optional error, and duration

---

## External Integrations

| System | Purpose | Protocol |
|---|---|---|
| None | N/A | N/A |

---

## Failure Scenarios

| Failure | Impact | Handling Strategy |
|---|---|---|
| Malformed JSON body | Create/update cannot be parsed | Return 400 with `VALIDATION_ERROR` |
| Missing or invalid task fields | Request violates business rules | Return 400 with `VALIDATION_ERROR` |
| Empty task ID | Task lookup/update/delete cannot target a task | Return 400 with `VALIDATION_ERROR` |
| Unknown task ID | Operation cannot complete | Return 404 with `NOT_FOUND` |
| SQLite/storage error | Request cannot complete | Return 500 with `INTERNAL_SERVER_ERROR` |
| Unsupported HTTP method | Route exists but method is not allowed | Return 405 Method Not Allowed |
| Concurrent modification | Last successful write determines stored task state | Repository relies on SQLite transaction/locking behavior |

---

## Trade-offs

List architectural decisions:

- Monolith vs microservices: Monolith chosen for simplicity and local development speed
- SQLite vs external database server: SQLite chosen for durable local storage with minimal operational setup
- Go standard library vs web framework: `net/http` chosen to avoid framework dependency and keep behavior explicit
- Synchronous request flow vs background jobs: Synchronous chosen because all operations are immediate CRUD actions
- Single-user API vs multi-user system: Single-user chosen for MVP, with no user isolation or authorization model

---

## Constraints Applied

Explain how constraints influenced architecture.

- **No authentication**: Routes focus only on task resources and validation
- **Simple storage**: Repository uses SQLite as an embedded database
- **CRUD operations**: Handler-Service-Repository flow maps directly to create/list/get/update/delete use cases
- **Durable task data**: API is stateless with respect to sessions, and tasks are stored in a local SQLite database file
- **No external integrations**: Startup only wires local dependencies and starts an HTTP server on port `8080`

---

## Key Questions (AI must validate)

- Are responsibilities well separated? Yes, handler, service, repository, validator, middleware, and logger are distinct
- Is the architecture over-engineered? No, it uses only local packages and the Go standard library
- Are failure points addressed? Yes, validation, not found, internal errors, and unsupported methods are covered
- Is there a simpler alternative? The current design is already minimal while preserving testable layers
