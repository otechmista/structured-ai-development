# Architecture Checklist

Use this checklist before moving from Container View to Component View.

## Architecture Style

- [ ] The selected style is explicit.
- [ ] The style is justified by context and constraints.
- [ ] The architecture is not over-engineered.

## System Blocks

- [ ] Major blocks are listed.
- [ ] Each block has one clear responsibility.
- [ ] Boundaries between blocks are clear.
- [ ] No block duplicates another block's responsibility.

## Data Flow

- [ ] Main data flow is described step by step.
- [ ] Inputs and outputs between blocks are clear.
- [ ] Storage responsibilities are clear.
- [ ] Side effects are identified.

## External Dependencies

- [ ] External systems are listed.
- [ ] Protocols are listed.
- [ ] Dependency failures are considered.

## Failure Scenarios

- [ ] Major failure scenarios are listed.
- [ ] Impact is described.
- [ ] Handling strategy is defined.

## Readiness

- [ ] The architecture satisfies the Context View.
- [ ] Trade-offs are documented.
- [ ] The system is ready for component expansion.
