# Code View and Generation Prompt

You are helping define the Code View and generate implementation only after the previous views are complete.

Input:

- `01_CONTEXT_VIEW.md`
- `02_CONTAINER_VIEW.md`
- `03_COMPONENT_VIEW.md`
- `04_CODE_VIEW.md`
- Existing source code, if any.

Goal:

- Implement code that follows the defined views exactly.
- Produce tests for business rules, edge cases, and failure scenarios.

Rules:

- Use Caveman mode: short, direct, no filler.
- Generate code only from documented requirements.
- Do not invent business rules.
- Do not change architecture.
- Keep layers separated.
- Keep code simple.
- Include error handling.
- Include tests.
- List assumptions explicitly.

Tasks:

1. Validate that all previous views are complete.
2. Confirm project structure and implementation rules.
3. Implement entities, handlers, services, repositories, validators, or other defined components.
4. Implement only the API contracts defined in the Code View.
5. Add error handling according to the Code View.
6. Add logging if required.
7. Add tests for rules, edge cases, and failure scenarios.
8. Run available validation commands.
9. Report assumptions and any deviations.

Output:

- Files changed.
- Tests added or updated.
- Validation results.
- Assumptions.
- Any unresolved gaps.
