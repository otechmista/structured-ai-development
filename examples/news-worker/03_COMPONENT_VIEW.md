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

Worker Entrypoint

---

## Components

| Component | Responsibility |
|---|---|
| ConfigLoader | Reads and validates runtime configuration |
| WorkerRunner | Coordinates one complete worker run |
| GNewsClient | Fetches articles from GNews |
| ArticleNormalizer | Converts API articles into internal records |
| ArticleDeduplicator | Removes invalid and repeated articles |
| ArticleRepository | Persists articles and run metadata |
| RunLogger | Logs duration, counts, and errors |

---

## Component Specifications

### Component: ConfigLoader

Responsibility:

Load required settings from environment or platform configuration.

Inputs:

- Environment variables

Outputs:

- Validated worker configuration

Business Rules:

- API key is required.
- Fetch mode must be `top-headlines` or `search`.
- Max articles per request must not exceed the configured free-plan limit.
- Schedule frequency must be conservative enough for configured daily quota.

Validations:

- API key exists.
- Language and country values are allowed by the API configuration.
- Max articles is a positive integer.
- Query is required when fetch mode is `search`.

Error Cases:

- Missing API key
- Invalid fetch mode
- Invalid article limit
- Missing search query

Dependencies:

- Runtime environment

Does NOT do:

- Call external APIs

### Component: GNewsClient

Responsibility:

Request recent news from GNews and return raw API articles.

Inputs:

- Worker configuration
- Fetch mode
- Query, language, country, topic, and max article settings

Outputs:

- Raw GNews response
- API error result

Business Rules:

- API key must be sent through supported GNews authentication.
- Client must not retry 401, 403, or rate-limit responses in the same run.
- Client may retry transient network failures with bounded retry policy.

Validations:

- Endpoint must match fetch mode.
- Response must contain an articles array.

Error Cases:

- Unauthorized
- Rate limited
- Network timeout
- Malformed response

Dependencies:

- HTTP client
- ConfigLoader output

Does NOT do:

- Persist articles
- Deduplicate records

### Component: ArticleNormalizer

Responsibility:

Convert raw GNews article objects into internal Article records.

Inputs:

- Raw article list

Outputs:

- Normalized Article records
- Rejected article count

Business Rules:

- Articles without title or URL must be discarded.
- Source attribution must be preserved.
- Published date must be converted to a normalized timestamp.
- API-provided description may be stored as a snippet.

Validations:

- Title is present.
- URL is present and valid.
- Published date is parseable when provided.
- Source name is preserved when available.

Error Cases:

- Missing required fields
- Invalid URL
- Invalid publication date

Dependencies:

- None

Does NOT do:

- Fetch article web pages
- Generate summaries

### Component: ArticleDeduplicator

Responsibility:

Remove duplicate candidate articles before persistence.

Inputs:

- Normalized Article records
- Existing article URLs from repository

Outputs:

- New Article records only
- Duplicate count

Business Rules:

- Canonical URL is the deduplication key.
- Existing stored article wins over a duplicate candidate.
- Newer runs must not overwrite newer stored data with older API data.

Validations:

- Article URL must be canonicalized before comparison.

Error Cases:

- Repository lookup failure

Dependencies:

- ArticleRepository

Does NOT do:

- Decide editorial relevance

### Component: WorkerRunner

Responsibility:

Coordinate the full worker execution.

Inputs:

- Trigger event

Outputs:

- Run result with counts and status

Business Rules:

- A run is successful only if fetch and persistence complete.
- Partial rejection of invalid articles does not fail the run if valid articles were processed.
- Storage failure fails the run.

Validations:

- Configuration must be valid before execution.
- Run must record start and end time.

Error Cases:

- Configuration failure
- API failure
- Normalization failure
- Storage failure

Dependencies:

- ConfigLoader
- GNewsClient
- ArticleNormalizer
- ArticleDeduplicator
- ArticleRepository
- RunLogger

Does NOT do:

- Hide run failures

---

## Internal Flow

Describe execution:

1. WorkerRunner starts and records start time.
2. ConfigLoader validates configuration.
3. GNewsClient fetches raw articles.
4. ArticleNormalizer converts valid raw articles into internal records.
5. ArticleRepository loads existing URLs for the same configured scope.
6. ArticleDeduplicator removes duplicates.
7. ArticleRepository inserts new records.
8. ArticleRepository records run metadata.
9. RunLogger emits final status, counts, error, and duration.

---

## Edge Cases

List explicitly:

- API returns zero articles.
- API returns duplicate URLs in the same response.
- API returns a story without a URL.
- API key is missing in production.
- Worker is triggered twice at the same time.
- Storage insert partially succeeds.
- Free-plan daily quota is exhausted.

---

## Data Transformations

Describe how data changes:

- GNews raw article -> normalized Article -> deduplicated Article -> stored Article
- HTTP/API error -> worker error category -> run metadata and log event
- Environment values -> validated Config -> request parameters

---

## Key Questions (AI must validate)

- Are responsibilities clear?
- Are rules complete?
- Are validations sufficient?
- Are edge cases covered?
