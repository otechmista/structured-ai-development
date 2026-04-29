# CONTEXT VIEW
(Problem, Business Intent, Boundaries)

## Instructions for AI

- Do NOT generate code
- Do NOT assume missing information
- Ask questions if context is unclear
- Focus on understanding the problem

---

## System Name

News Worker

---

## Problem

Applications often need a small background process that periodically retrieves recent news, normalizes article data, removes duplicates, and stores a clean list for downstream use.

The system must define a worker that fetches recent news from GNews, a public news API with a free plan for development and testing.

---

## Users

List all actors:

- Human users:
  - Developer configuring the worker
  - Operator monitoring worker health
  - Product user indirectly consuming curated news
- Systems:
  - Scheduled worker runtime
  - News storage
  - Downstream application or API
- External integrations:
  - GNews API
  - Observability or logging service

---

## Goals

What must be achieved:

- Fetch recent headlines from a public news API.
- Normalize external article data into an internal format.
- Deduplicate articles across runs.
- Store successful fetch results for downstream consumption.
- Handle rate limits, authentication errors, and API failures explicitly.

---

## Non-Goals

What is explicitly NOT part of the system:

- Building a user-facing news website.
- Paying for premium news API features.
- Scraping websites directly.
- Performing sentiment analysis or summarization.
- Guaranteeing real-time breaking news.

---

## Business Rules (High Level)

Rules that affect behavior:

- Worker must use GNews API credentials from environment configuration.
- Worker must respect the free-plan request limits configured by the operator.
- Articles must be considered duplicates when they share the same canonical URL.
- Articles without title or URL must be discarded.
- Stored articles must include source attribution.
- Worker must not overwrite newer stored data with older fetch results.

---

## Constraints

- Technical:
  - Worker must run without user interaction.
  - API key must not be hard-coded.
  - Fetch, normalize, deduplicate, and persist steps must be separable.
- Business:
  - Free development/testing usage is preferred.
  - Request volume must be conservative.
- Legal:
  - Respect source attribution and API terms.
  - Store links and metadata, not full copied article bodies beyond API-provided snippets.
- Operational:
  - Failures must be logged with enough context to diagnose.
  - Repeated failures must not create duplicate records.

---

## System Boundaries

Inside the system:

- Scheduled worker execution
- GNews request construction
- Response validation
- Article normalization
- Deduplication
- Persistence of normalized article metadata
- Operational logging

Outside the system:

- GNews account management and billing
- News source publishing behavior
- Downstream UI rendering
- Full-text article extraction

---

## Key Questions (AI must validate)

- Is the problem clear?
- Are goals measurable?
- Are constraints realistic?
- Are there missing actors?
