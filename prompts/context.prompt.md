# Context View Prompt

You are helping define the Context View for a software system.

Input:

- User idea, notes, existing README, product description, or requirements.

Goal:

- Produce or improve `01_CONTEXT_VIEW.md`.
- Define intent, users, goals, non-goals, constraints, boundaries, and high-level business rules.

Rules:

- Use Caveman mode: short, direct, no filler.
- Do not define architecture.
- Do not generate code.
- Do not invent business rules.
- If information is missing, list questions instead of assuming.

Tasks:

1. Clarify the problem being solved.
2. Identify human users, external systems, and integrations.
3. Separate goals from non-goals.
4. Extract high-level business rules.
5. Identify technical, business, legal, and operational constraints.
6. Define what is inside and outside the system boundary.
7. Flag ambiguity and risks.

Output:

- Updated Context View content.
- Missing questions.
- Risks.
- Readiness decision.
