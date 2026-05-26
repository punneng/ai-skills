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

### Step 2 — Ask structured questions

Group ALL questions into ONE call to the Question tool (max 3 questions). Ask all three dimensions at once:

1. **Feature scope** — "The next phase is **{$NEXT_PHASE}**. Which parts should this feature cover, and what should we call the directory? (e.g. YYYY-MM-DD-feature-name)"
   - Offer 2-3 meaningful scope options based on `$PHASE_TASKS` (e.g. full phase, a subset, or a specific task)
   - Include a custom option for the user to type their own answer

2. **Key decisions & context** — "What key decisions and constraints should be recorded in requirements.md?"
   - Offer options based on what `tech-stack.md` and `mission.md` suggest (e.g. pin assignments, framework version, partition scheme, config format)
   - Include a custom option

3. **Validation criteria** — "What defines success for this feature? What validation criteria should go into validation.md?"
   - Offer options based on the roadmap milestone and phase tasks
   - Include a custom option

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

1. **Always use the Question tool** — never guess or fill in defaults for the user. Every answer must come from them.
2. **Group all questions** — batch all three into a single Question tool call.
3. **Read all context files first** — mission.md, tech-stack.md, and roadmap.md must be read before asking.
4. **Write all three files** — never skip a file, never write partial content.
5. **Work in `specs/`** — create the dated subdirectory under specs/.
6. **Follow tech-stack.md** — all technology decisions in the spec files must match what's in tech-stack.md.
7. **Be concise** — keep each file focused and readable. No fluff, no filler sections.
8. **Let the user steer** — if they give brief answers, write brief files. If they give detailed answers, match the depth.
