---
name: specify-feature
description: Create a feature spec (plan.md, requirements.md, validation.md) under specs/ for the next unstarted roadmap phase.
trigger: /specify-feature
---

# /specify-feature — Create a feature spec

Generate three spec files for the next unstarted phase in `specs/roadmap.md` — `plan.md`, `requirements.md`, and `validation.md` — by asking structured questions and writing the results to a dated directory under `specs/`.

## When to use

- User says: "create a feature spec", "spec out the next phase", "let's plan the next feature"
- Starting work on a new phase from the roadmap

## Flow

### Step 1 — Read context files

Read these three files before asking any questions:

1. `specs/roadmap.md` — identify the next unstarted phase (first phase whose milestone is not checked/completed)
2. `specs/mission.md` — understand project goals, non-goals, and design principles
3. `specs/tech-stack.md` — understand chosen components, frameworks, and constraints

Store the next phase name/title as `$NEXT_PHASE` and its bullet items as `$PHASE_TASKS`.

### Step 1.5 — Detect tool availability

Check if `grill-with-docs` skill is available (look for it in the available skills list or check if its SKILL.md exists at a known path).

- If **grill-with-docs is available**, invoke it for Step 2.
- If **grill-with-docs is NOT available**, fall back to the Question tool for Step 2.

### Step 2 — Grill to crystallize the spec

Cover three dimensions to resolve scope, decisions, and validation:

1. **Feature scope** — What should this feature cover, and what should the directory be called? (e.g. `YYYY-MM-DD-feature-name`)
2. **Key decisions & constraints** — What decisions, trade-offs, and constraints should go into `requirements.md`?
3. **Validation criteria** — What defines success? What should go into `validation.md`?

**If using grill-with-docs:** Invoke the grill-with-docs skill. Let the grill challenge assumptions, cross-reference with `mission.md` and `tech-stack.md`, and resolve ambiguities before moving on. Ask questions one at a time, waiting for feedback on each before continuing.

**If using Question tool:** Group questions into ONE call to the Question tool (3 questions max per call). Provide meaningful options for each, plus the ability to type custom answers.

### Step 3 — Create directory

Create a directory at `specs/{YYYY-MM-DD-feature-name}/` using the name from the user's answer.

### Step 4 — Generate files

Write three files:

**`specs/{dir}/plan.md`** — Series of numbered task groups:
- Group related tasks under sub-headings
- Each group covers a logical step (setup, implementation, verification)
- Use `- [ ]` checklist format for individual tasks
- Reference the roadmap phase tasks and tech-stack components

**`specs/{dir}/requirements.md`** — Scope, decisions, and context:
- **Scope**: What this feature covers and explicitly does not cover
- **Key Decisions table**: Component/Choice/Rationale rows, grounded in tech-stack.md
- **Constraints**: Non-negotiable limits (build must pass, no extra hardware, etc.)

**`specs/{dir}/validation.md`** — How to know implementation succeeded:
- **Acceptance Criteria**: Checklist of verifiable conditions
- Grouped by category (Build, Flash, Hardware, Environment)
- **How to Test**: Exact commands or steps to run
- Must produce a clear pass/fail on each criterion

### Step 5 — Confirm

Print a summary of what was created:
```
Created specs/{dir}/
├── plan.md
├── requirements.md
└── validation.md
```

Mention that the spec is ready for implementation.

## Behavior rules

1. **Detect tool availability** — check if grill-with-docs is available at start. If available, use it. If not, fall back to Question tool.
2. **Never guess defaults** — every answer must come from the user.
3. **Read all context files first** — mission.md, tech-stack.md, and roadmap.md must be read before starting the grill.
4. **Write all three files** — never skip a file, never write partial content.
5. **Work in `specs/`** — create the dated subdirectory under specs/.
6. **Follow tech-stack.md** — all technology decisions in the spec files must match what's in tech-stack.md.
7. **Be concise** — keep each file focused and readable. No fluff, no filler sections.
8. **Let the user steer** — if they give brief answers, write brief files. If they give detailed answers, match the depth.
