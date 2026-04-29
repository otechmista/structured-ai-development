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

TypeScript running on Node.js

---

## Project Structure

Describe the folder and file organization:

```txt
src/
+-- config/
|   +-- load-config.ts
+-- gnews/
|   +-- client.ts
|   +-- types.ts
+-- articles/
|   +-- article.ts
|   +-- normalize.ts
|   +-- deduplicate.ts
|   +-- repository.ts
+-- worker/
|   +-- runner.ts
|   +-- run-result.ts
+-- logging/
|   +-- logger.ts
+-- index.ts
tests/
+-- config.test.ts
+-- gnews-client.test.ts
+-- normalize.test.ts
+-- deduplicate.test.ts
+-- runner.test.ts
```

---

## Entities

Define core entities:

| Entity | Description |
|---|---|
| WorkerConfig | Validated runtime settings for the worker |
| RawGNewsArticle | Article shape returned by GNews |
| Article | Internal normalized news article record |
| RunResult | Final worker result with status, counts, error, and duration |
| RunMetadata | Persisted metadata for each worker run |
| WorkerError | Categorized error used for logging and run status |

---

## API Contracts

List endpoints:

| Method | Path | Description |
|---|---|---|
| GET | https://gnews.io/api/v4/top-headlines | Fetch top headlines by language, country, topic, and max |
| GET | https://gnews.io/api/v4/search | Search articles by query, language, country, and max |

Required environment variables:

| Name | Description |
|---|---|
| GNEWS_API_KEY | API key for GNews |
| NEWS_FETCH_MODE | `top-headlines` or `search` |
| NEWS_QUERY | Search query, required for search mode |
| NEWS_LANG | Language filter |
| NEWS_COUNTRY | Country filter |
| NEWS_TOPIC | Optional headline topic |
| NEWS_MAX | Max articles per request |
| NEWS_DAILY_REQUEST_LIMIT | Operator-defined daily request guardrail |

---

## Implementation Rules

Enforce architecture:

- Keep configuration validation in ConfigLoader.
- Keep HTTP access inside GNewsClient.
- Keep normalization pure and independently testable.
- Keep deduplication separate from persistence.
- Keep repository behind an interface so storage can change.
- Do not hard-code API keys.
- Do not scrape article pages.
- Do not treat a failed persistence step as success.

---

## Error Handling

Define error categories:

| Error Type | Description | Handling |
|---|---|---|
| Validation errors | Invalid configuration or article fields | Fail fast for config, reject invalid articles during normalization |
| Business errors | Rate limit, quota exhausted, duplicate articles, unsupported fetch mode | Log with category and stop or skip according to rule |
| System errors | Network timeout, malformed API response, storage unavailable | Retry only transient fetch failures, fail run on storage failure |

---

## Logging

Must include:

- request_id
- operation
- error
- duration

Worker logs must also include:

- fetched_count
- normalized_count
- rejected_count
- duplicate_count
- inserted_count
- fetch_mode

Logs must not include `GNEWS_API_KEY`.

---

## Tests

Required:

- Business rules
- Edge cases
- Failure scenarios

Specific tests:

- ConfigLoader fails when `GNEWS_API_KEY` is missing.
- ConfigLoader requires `NEWS_QUERY` in search mode.
- GNewsClient maps 401 and 403 responses to non-retryable errors.
- ArticleNormalizer rejects articles without title or URL.
- ArticleNormalizer preserves source attribution.
- ArticleDeduplicator removes duplicate canonical URLs.
- WorkerRunner fails the run when storage insert fails.
- WorkerRunner records zero-article runs without inserting duplicates.

---

## Performance & Security Constraints

- Performance:
  - Use bounded request size.
  - Avoid loading unbounded article history for deduplication.
  - Use indexed URL lookup in persistent storage.
- Scalability:
  - Keep each run idempotent by URL.
  - Allow repository implementation to evolve from local storage to database.
- Security:
  - API key must come from environment or secret manager.
  - Do not log secrets.
  - Store article metadata and URLs, not copied full article bodies.

---

## Key Questions (AI must validate)

- Does code match architecture?
- Are rules implemented?
- Are edge cases handled?
- Is complexity justified?
