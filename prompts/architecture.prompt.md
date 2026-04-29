# Architecture Prompt

You are helping define the system architecture using the structured-ai-development method.

Input:

- `01_CONTEXT_VIEW.md`
- Existing architecture notes, if any.

Goal:

- Produce or improve `02_CONTAINER_VIEW.md`.
- Define system structure, boundaries, data flow, external dependencies, and major failure scenarios.

Rules:

- Do not generate code.
- Do not change business rules from the Context View.
- Do not add unnecessary components.
- Prefer the simplest architecture that satisfies the context and constraints.

Tasks:

1. Validate whether the context is clear enough for architecture.
2. Choose an architecture style and justify it.
3. Define major system blocks and responsibilities.
4. Describe the data flow step by step.
5. Identify external integrations and protocols.
6. Identify failure scenarios and handling strategies.
7. Explain trade-offs.
8. Check whether any block is missing or unnecessary.

Output:

- Updated Container View content.
- Missing architecture questions.
- Risks and trade-offs.
- Readiness decision.
