# structured-ai-development

A practical method for building software with AI without letting code generation outrun system design.

LLMs are useful at producing code, summaries, tests, and alternatives. They are not a substitute for clear requirements, explicit architecture, business rules, or engineering judgment. This repository gives you a repeatable workflow for using AI as an assistant across the whole development process, not just at the final coding step.

---

## What this method does

The method separates thinking from implementation through four progressive views:

1. **Context View** - define the problem, users, goals, non-goals, constraints, and high-level rules.
2. **Container View** - define the main system blocks, boundaries, data flow, and external dependencies.
3. **Component View** - define component responsibilities, detailed behavior, validations, and edge cases.
4. **Code View** - define implementation structure, API contracts, entities, error handling, tests, and coding rules.

Code generation happens only after the first three views are complete and validated.

---

## Why use it

Most AI-assisted development fails in predictable ways:

- vague prompts produce vague systems
- generated code mixes architecture, behavior, and implementation details
- business rules get invented or forgotten
- early speed turns into maintenance cost
- reviews become harder because there is no source of truth

This method creates that source of truth before asking AI to write code.

---

## Repository structure

```txt
.
+-- README.md                         # Project overview and usage guide
+-- METHOD.md                         # Full explanation of the four-view method
+-- AI_WORKFLOW.md                    # Rules for how AI agents should execute the method
+-- templates/
|   +-- 01_CONTEXT_VIEW.md            # Blank template for intent and boundaries
|   +-- 02_CONTAINER_VIEW.md          # Blank template for system structure
|   +-- 03_COMPONENT_VIEW.md          # Blank template for behavior and rules
|   +-- 04_CODE_VIEW.md               # Blank template for implementation contracts
+-- prompts/
|   +-- how-to-use.prompt.md          # General AI operating prompt
|   +-- context.prompt.md             # Context View prompt
|   +-- architecture.prompt.md        # Container/architecture prompt
|   +-- component.prompt.md           # Component View prompt
|   +-- code.prompt.md                # Code generation prompt
|   +-- review.prompt.md              # Review prompt
+-- checklists/
|   +-- method-readiness.md           # Decide whether the method fits a project
|   +-- context-checklist.md          # Validate Context View readiness
|   +-- architecture-checklist.md     # Validate Container View readiness
|   +-- component-checklist.md        # Validate Component View readiness
|   +-- code-review.md                # Validate generated implementation
+-- examples/
    +-- todo-api/                     # Worked TODO API specification
    |   +-- 01_CONTEXT_VIEW.md
    |   +-- 02_CONTAINER_VIEW.md
    |   +-- 03_COMPONENT_VIEW.md
    |   +-- 04_CODE_VIEW.md
    +-- thweads-platform/             # Worked SvelteKit social platform specification
    |   +-- 01_CONTEXT_VIEW.md
    |   +-- 02_CONTAINER_VIEW.md
    |   +-- 03_COMPONENT_VIEW.md
    |   +-- 04_CODE_VIEW.md
    +-- news-worker/                  # Worked news-fetching worker specification
        +-- 01_CONTEXT_VIEW.md
        +-- 02_CONTAINER_VIEW.md
        +-- 03_COMPONENT_VIEW.md
        +-- 04_CODE_VIEW.md
```

---

## Quick start

### 1. Check if the method fits

Start with:

```txt
checklists/method-readiness.md
```

Use this method when the system has meaningful behavior, business rules, multiple parts, or is expected to evolve. Skip it for tiny scripts, disposable prototypes, and trivial tasks.

### 2. Create the four view files

Copy the templates into your project:

```txt
templates/01_CONTEXT_VIEW.md
templates/02_CONTAINER_VIEW.md
templates/03_COMPONENT_VIEW.md
templates/04_CODE_VIEW.md
```

Keep the file order. Each view depends on the decisions made in the previous one.

### 3. Complete the Context View

Use:

```txt
prompts/context.prompt.md
checklists/context-checklist.md
```

Define the problem, users, goals, non-goals, constraints, system boundaries, and high-level rules. Do not discuss implementation yet.

### 4. Complete the Container View

Use:

```txt
prompts/architecture.prompt.md
checklists/architecture-checklist.md
```

Define the system blocks, data flow, dependencies, and architectural decisions. Do not define internal component behavior yet.

### 5. Complete the Component View

Use:

```txt
prompts/component.prompt.md
checklists/component-checklist.md
```

This is the critical behavior layer. Define responsibilities, validations, business rules, edge cases, and dependencies for each component.

### 6. Complete the Code View

Use:

```txt
prompts/code.prompt.md
```

Define implementation structure, contracts, entities, errors, tests, and constraints. Only now should AI generate code.

### 7. Review the output

Use:

```txt
prompts/review.prompt.md
checklists/code-review.md
```

Review generated code against the four views. Treat AI output as a draft, not as authority.

---

## Examples

The `examples/` directory contains worked specifications:

```txt
examples/todo-api/
examples/thweads-platform/
examples/news-worker/
```

Each example demonstrates the full progression from problem definition to implementation contract:

```txt
Context -> Container -> Component -> Code
```

The examples are intentionally documentation-first. They show what should be true before code is generated.

---

## Recommended AI workflow

When using an AI assistant, start by giving it:

```txt
prompts/how-to-use.prompt.md
AI_WORKFLOW.md
```

Then work one view at a time. The AI should:

- read the current view and all previous views
- identify missing information
- ask only necessary questions
- suggest improvements
- validate readiness before moving forward
- stop if requirements, rules, or architecture are unclear

Expected AI role by phase:

| Phase | AI role |
|---|---|
| Context | Analyst |
| Container | Architect |
| Component | Engineer |
| Code | Developer |
| Review | Reviewer |

---

## Core rules

- Do not generate code before Context, Container, and Component views are complete.
- Do not let AI invent business rules.
- Do not merge architecture, behavior, and implementation into one prompt.
- Do not skip validation gates.
- Do not accept generated code without review.
- Keep every important decision explicit.

---

## Principles

- Clarity before speed
- Structure before code
- Behavior before implementation
- Prompts are specifications, not casual conversations
- AI output is not authority
- Engineering judgment remains required

---

## Trade-offs

This method is deliberately slower at the beginning. It requires discipline, explicit documentation, and review. In exchange, it reduces ambiguity, makes AI output easier to evaluate, and gives teams a shared design trail as the system evolves.

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE).
