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

- modular monolith

Justify the choice.

The worker is a small background application with clear internal modules: scheduler entrypoint, API client, normalization, deduplication, repository, and logging. Splitting it into services would add unnecessary operational complexity.

---

## System Blocks

List all major blocks:

| Block | Responsibility | Technology (optional) |
|---|---|---|
| Scheduler | Triggers worker runs on a configured interval | Cron, platform scheduler, or queue trigger |
| Worker Entrypoint | Coordinates one fetch-normalize-store cycle | Node.js, Python, Go, or runtime of choice |
| GNews Client | Calls GNews API and handles API-level errors | HTTPS JSON |
| Normalizer | Converts GNews articles into internal Article records | Application module |
| Deduplicator | Removes duplicates by canonical URL | Application module |
| Repository | Stores and reads article records and run metadata | Database or object storage |
| Logger | Emits operational logs and metrics | Console or observability service |

---

## Data Flow

Describe step-by-step:

1. Scheduler starts a worker run.
2. Worker loads configuration from environment.
3. Worker validates required settings such as API key, query, language, country, max articles, and rate limit policy.
4. GNews Client requests top headlines or search results from GNews.
5. Client validates HTTP status and response shape.
6. Normalizer converts each external article into an internal Article record.
7. Deduplicator removes invalid and duplicate records.
8. Repository compares candidate articles with stored URLs.
9. Repository inserts only new articles and records run metadata.
10. Logger emits success, partial failure, or failure result.

---

## External Integrations

| System | Purpose | Protocol |
|---|---|---|
| GNews API | Retrieve recent news articles and top headlines | HTTPS JSON |
| News Storage | Persist normalized article metadata | Database protocol or storage SDK |
| Observability Service | Track worker runs, errors, and duration | HTTPS or stdout collector |

---

## Failure Scenarios

| Failure | Impact | Handling Strategy |
|---|---|---|
| Missing API key | Worker cannot authenticate | Fail fast before making requests |
| GNews returns 401 | Worker cannot fetch news | Log auth error and stop run |
| GNews returns 403 or rate limit | Worker cannot continue within limits | Log rate limit and skip retry until next schedule |
| GNews returns malformed data | Articles cannot be trusted | Reject malformed records and log count |
| Storage unavailable | New articles cannot be persisted | Do not mark run as successful |
| Duplicate articles | Storage may become noisy | Deduplicate by canonical URL before insert |

---

## Trade-offs

List architectural decisions:

- Use a single worker process to keep operational overhead low.
- Use GNews because it provides JSON news data and a free development/testing plan.
- Store metadata and URLs instead of copying full article content.
- Prefer conservative scheduling to avoid exhausting free-plan quotas.

---

## Constraints Applied

The architecture isolates API access, normalization, deduplication, and persistence so each behavior can be tested independently. API credentials are injected through environment variables.

---

## Key Questions (AI must validate)

- Are responsibilities well separated?
- Is the architecture over-engineered?
- Are failure points addressed?
- Is there a simpler alternative?
