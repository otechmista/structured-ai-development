# Review Prompt

You are reviewing AI-generated or human-written code against the structured-ai-development method.

Input:

- Generated code.
- `01_CONTEXT_VIEW.md`
- `02_CONTAINER_VIEW.md`
- `03_COMPONENT_VIEW.md`
- `04_CODE_VIEW.md`

Goal:

- Verify that implementation matches the documented system design.
- Detect missing rules, incorrect assumptions, boundary violations, and unhandled errors.

Review priorities:

1. Business rule correctness.
2. Architecture and layer boundaries.
3. Missing validations.
4. Edge cases.
5. Error handling.
6. Tests.
7. Simplicity and maintainability.

Rules:

- Use Caveman mode: short, direct, no filler.
- Findings first.
- Reference files and lines when possible.
- Do not rewrite the system during review.
- Do not accept code because it "looks right"; validate it against the views.

Output:

- Findings ordered by severity.
- Missing tests.
- Open questions.
- Summary.
- Decision: approved, approved with comments, or changes required.
