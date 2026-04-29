# METHOD

## What this is

A structured approach to building software systems with AI.
The method separates thinking from coding.
AI is used as an assistant, not as a decision-maker.

---

## Core Idea

AI fails when asked to do everything at once.

This method enforces separation of concerns through four views:

1. Context View
2. Container View
3. Component View
4. Code View

Each view refines the system before moving to the next.

---

## The Four Views

### 1. Context View

Defines intent.

Answers:

- What problem are we solving?
- Who are the users?
- What are the goals?
- What is out of scope?
- What are the high-level business rules?

No architecture.
No code.

---

### 2. Container View

Defines structure.

Answers:

- What are the main system blocks?
- How do they communicate?
- What is the data flow?
- What integrations exist?
- What architectural decisions were made?

No internal logic.
No code.

---

### 3. Component View

Defines behavior.

Answers:

- What does each part do?
- What are the business rules in detail?
- What validations exist?
- What are the edge cases?
- How does data move internally?

This is the most critical layer.

No full implementation.

---

### 4. Code View

Defines implementation.

Answers:

- What will be coded?
- How is the project structured?
- What are the API contracts?
- What are the entities?
- What are the implementation rules?

Code is generated only here.

---

## Rule

Never generate code before completing the first three views.

---

## Why this works

- Reduces ambiguity
- Prevents AI hallucination
- Forces explicit decisions
- Improves consistency
- Makes systems easier to evolve

---

## Trade-offs

- Requires discipline
- Slower initial setup
- More documentation upfront

---

## When to use

- APIs with business rules
- Systems with multiple components
- Long-term projects
- Teams using AI regularly

---

## When NOT to use

- Small scripts
- Throwaway code
- Simple prototypes

---

## Principles

- Clarity before speed
- Structure before code
- Behavior before implementation
- AI is not authority
- Every decision must be explicit

---

## Common Failure Modes

- Skipping Context View
- Mixing architecture with code
- Letting AI define business rules
- Over-engineering early
- Ignoring edge cases

---

## Final Note

AI can generate code.
It cannot define systems.
This method exists to close that gap.