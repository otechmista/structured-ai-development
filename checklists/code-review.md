# Code Review Checklist

Use this checklist to review generated or human-written code.

## Alignment With Views

- [ ] Code follows `01_CONTEXT_VIEW.md`.
- [ ] Code follows `02_CONTAINER_VIEW.md`.
- [ ] Code follows `03_COMPONENT_VIEW.md`.
- [ ] Code follows `04_CODE_VIEW.md`.
- [ ] Any deviation is documented and justified.

## Architecture Boundaries

- [ ] Handlers do not contain business logic.
- [ ] Services do not contain transport-specific logic.
- [ ] Repositories do not contain business decisions.
- [ ] Validators do not perform unrelated side effects.
- [ ] External integrations are isolated.

## Business Rules

- [ ] All documented rules are implemented.
- [ ] No undocumented rule was added.
- [ ] Rule conflicts are handled or documented.

## Validation and Errors

- [ ] Input validation is implemented.
- [ ] Business errors are handled.
- [ ] System errors are handled.
- [ ] Error responses do not leak sensitive details.
- [ ] Failure scenarios from the architecture are covered.

## Tests

- [ ] Business rules are tested.
- [ ] Edge cases are tested.
- [ ] Failure scenarios are tested.
- [ ] Tests are deterministic.
- [ ] Tests match documented behavior.

## Maintainability

- [ ] Code is simple.
- [ ] Names reflect documented concepts.
- [ ] Complexity is justified.
- [ ] There are no hidden side effects.
- [ ] Assumptions are documented.

## Decision

- [ ] Approved.
- [ ] Approved with comments.
- [ ] Changes required.
