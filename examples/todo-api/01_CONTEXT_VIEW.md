# CONTEXT VIEW - TODO API
(Problem, Business Intent, Boundaries)

## Instructions for AI

- Do NOT generate code
- Do NOT assume missing information
- Ask questions if context is unclear
- Focus on understanding the problem

---

## System Name

TODO Management API

---

## Problem

Users need a small HTTP API to manage personal tasks/todos locally. The current implementation provides a centralized API for creating, listing, reading, updating, and deleting daily tasks without authentication or external dependencies.

Constraints:
- Must support basic CRUD operations over HTTP
- Single user context, with no authentication or authorization
- Tasks are personal and not shared
- Simple priority levels: low, medium, high
- Simple status values: pending, done
- Data is persisted in a local SQLite database

---

## Users

List all actors:

- Human users: Individual task managers using an HTTP client, REST client, browser-based client, or test client
- Systems: Web/mobile/CLI clients consuming the API over HTTP
- External integrations: None

---

## Goals

What must be achieved:

- Allow users to create new tasks
- Allow users to view all tasks
- Allow users to view one task by ID
- Allow users to update task details partially
- Allow users to delete tasks
- Track task completion status
- Return predictable JSON responses and HTTP status codes
- Keep the implementation simple enough for local development and tests

---

## Non-Goals

What is explicitly NOT part of the system:

- Multi-user support
- User authentication/authorization
- Real-time collaboration
- Task recurrence/scheduling
- Task dependencies/relationships
- File attachments
- Comments/notes on tasks beyond the task description field
- Pagination, filtering, or sorting options exposed through the API

---

## Business Rules (High Level)

Rules that affect behavior:

- A task must have a title
- A title must not be empty or whitespace-only
- A title must not have leading or trailing spaces
- A task can have a description
- A task has a priority level: low, medium, high
- A task defaults to medium priority when priority is omitted
- A task has a completion status: pending or done
- A task defaults to pending status when status is omitted
- All tasks are stored with a unique identifier
- Created and updated timestamps are managed by the repository
- Tasks can be marked as done independently by updating status

---

## Constraints

- Technical: Go HTTP API using the standard library `net/http`
- Technical: SQLite is the database used for task persistence
- Technical: The API keeps no session state; persistent task data is stored in SQLite
- Business: No concurrent task editing workflow; concurrent writes use last-write-wins behavior
- Legal: No sensitive user data is expected
- Operational: Data must survive process restarts through the SQLite database file

---

## System Boundaries

Inside the system:

- Task CRUD operations
- Task validation
- Task status management
- SQLite task storage
- JSON request/response handling
- Error response formatting
- Operation logging

Outside the system:

- User authentication
- User management
- External database servers
- Task synchronization across devices
- Real-time notifications
- Advanced scheduling
- External integrations

---

## Key Questions (AI must validate)

- Is the problem clear? Yes, simple personal task management over HTTP
- Are goals measurable? Yes, CRUD endpoints and response behavior are defined
- Are constraints realistic? Yes, no auth and SQLite storage fit the MVP
- Are there missing actors? No, only direct API clients are in scope
