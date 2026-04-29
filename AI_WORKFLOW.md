# AI WORKFLOW

## Purpose

Define how AI agents must use the method.

This is an execution pipeline, not a guideline.

---

## Global Rules

- Do NOT skip steps
- Do NOT generate code early
- Do NOT invent business rules
- Do NOT change architecture without justification
- Always validate before moving forward

---

## Step 1 — Context Analysis

Input:

- CONTEXT_VIEW.md

AI must:

- Understand the problem
- Identify missing information
- Identify risks
- Ask clarifying questions


Do NOT proceed if context is unclear.

---

## Step 2 — Architecture Definition

Input:

* CONTEXT_VIEW.md
* CONTAINER_VIEW.md

AI must:

* Validate architecture choices
* Identify missing blocks
* Identify unnecessary complexity
* Check data flow

Do NOT proceed if architecture is inconsistent.

---

## Step 3 — Component Expansion

Input:

* CONTEXT_VIEW.md
* CONTAINER_VIEW.md
* COMPONENT_VIEW.md

AI must:

* Expand component responsibilities
* Validate business rules
* Identify missing validations
* Identify edge cases

Do NOT proceed if behavior is incomplete.

---

## Step 4 — Code Generation

Input:

* CONTEXT_VIEW.md
* CONTAINER_VIEW.md
* COMPONENT_VIEW.md
* CODE_VIEW.md

AI must:

* Follow structure strictly
* Implement only defined rules
* Respect boundaries between layers
* Include error handling
* Include tests

Output:

* Source code
* Test code
* Assumptions list

---

## Step 5 — Review

Input:

* Generated code
* CONTEXT_VIEW.md
* CONTAINER_VIEW.md
* COMPONENT_VIEW.md
* CODE_VIEW.md

AI must:

* Verify alignment with views
* Detect missing rules
* Detect incorrect assumptions
* Detect unhandled errors

---

## Execution Flow

```text
Context → Validate
Container → Validate
Component → Validate
Code → Generate
Review → Validate
```

---

## Stop Conditions

AI must STOP if:

* Context is incomplete
* Business rules are unclear
* Architecture is inconsistent
* Components are undefined

---

## Anti-Patterns

Do NOT:

* Generate code from vague prompts
* Merge steps
* Assume missing requirements
* Introduce new abstractions without reason
* Optimize before correctness

---

## Expected Behavior

AI should behave as:

* Analyst in Context
* Architect in Container
* Engineer in Component
* Developer in Code
* Reviewer in Review

---

## Final Note

This workflow enforces thinking before coding.

Speed without structure leads to fragile systems.

