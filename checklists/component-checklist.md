# Component Checklist

Use this checklist before moving from Component View to Code View.

## Responsibilities

- [ ] Each component has a clear responsibility.
- [ ] Responsibilities do not overlap.
- [ ] Each component states what it does not do.

## Inputs and Outputs

- [ ] Inputs are defined.
- [ ] Outputs are defined.
- [ ] Data ownership is clear.
- [ ] Data transformations are described.

## Business Rules

- [ ] Rules from the Context View are expanded.
- [ ] No undocumented rule was introduced.
- [ ] Conflicting rules are flagged.

## Validations

- [ ] Required fields are identified.
- [ ] Format validations are identified.
- [ ] Business validations are identified.
- [ ] Authorization or permission checks are identified, if relevant.

## Error Cases

- [ ] Validation errors are listed.
- [ ] Business errors are listed.
- [ ] System errors are listed.
- [ ] Recovery or response behavior is described.

## Edge Cases

- [ ] Empty input cases are listed.
- [ ] Duplicate cases are listed, if relevant.
- [ ] Invalid state transitions are listed, if relevant.
- [ ] External dependency edge cases are listed.

## Readiness

- [ ] Component behavior is complete enough to implement.
- [ ] Open questions are documented.
- [ ] The system is ready for Code View.
