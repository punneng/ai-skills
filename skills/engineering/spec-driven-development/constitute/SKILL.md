---
name: constitute
description: Create a project constitution (mission.md, tech-stack.md, roadmap.md) in specs/ using structured questions and answers.
trigger: /constitute
---

# /constitute — Create a project constitution

Generate three foundational spec files in `specs/` — `mission.md`, `tech-stack.md`, and `roadmap.md` — by asking structured questions and writing the results to disk.

## When to use

- Starting a new project or need to document an existing project's direction
- Need to align on goals, tech choices, and implementation order
- User says: "create a constitution", "create specs", "let's plan the project"

## Flow

### Step 0 — Detect project state

Use Glob to check if the project root has substantive files beyond common scaffolding:

```
Glob pattern: *  (top-level)
Glob pattern: **/*.{py,rs,go,js,ts,tsx,java,c,cpp,h,swift,kt,cargo.toml,package.json,Cargo.toml,go.mod,build.gradle,pom.xml}
```

- If **no source files or config files** exist (only `.gitignore`, `README.md`, `.git/`, etc.), treat as a **new blank project**.
- If **source or project config files exist**, treat as an **existing project**.

### Step 1a — New blank project: Get project overview

Ask the user for a high-level project overview. Use the Question tool with a single open question:

```
question: "Tell me about your project at a high level. What are you building, and what problem does it solve?"
header: "Project overview"
options: []  # no predefined options — let them type freely
```

Wait for their answer and store it as `$PROJECT_OVERVIEW`.

Then proceed to Step 2.

### Step 1b — Existing project: Analyze & extract

Analyze the existing project to extract as much context as possible:

1. **Read key config files** — `package.json`, `Cargo.toml`, `go.mod`, `requirements.txt`, `pyproject.toml`, `CMakeLists.txt`, `Makefile`, `Dockerfile`, `platformio.ini`, `sdkconfig`, etc. (whichever exist)
2. **Read source tree structure** — use `read` on `src/`, `lib/`, `app/`, `components/`, etc. if they exist
3. **Check for existing docs** — `README.md`, `docs/`, `CONTRIBUTING.md`, `ARCHITECTURE.md`

Present a concise summary of what was detected (language, framework, build system, project type) and ask structured questions for any gaps. Group into ONE Question tool call:

```
question: "I detected a [language/framework] project. I still need: [list missing info]. Can you fill in the gaps?"
header: "Project analysis"
options: [...]  # context-aware options based on detection
```

Wait for their answer and merge it with the detected info to form `$PROJECT_OVERVIEW`.

### Step 2 — Ask structured questions

Group all questions into ONE call to the Question tool (3 questions max per call). Use the detection context to tailor the questions:

**If new blank project**, ask generic questions:
1. **Mission** — "What services will this project monitor/check? What triggers a notification? How should it behave when things go wrong (no WiFi, API down, empty results)?"
2. **Tech stack** — "What microcontroller, framework, display, connectivity, sensors, and output peripherals are you planning? What are your constraints (power, cost, size)?"
3. **Roadmap** — "How many implementation phases do you want? What is the minimal first deliverable? What goes in each subsequent phase?"

**If existing project**, ask targeted questions based on what was already detected:
1. **Mission** — "I see you're building [detected project type]. What are the core goals and non-goals for v1? What's the one-line purpose?"
2. **Tech stack** — "I detected [detected tech]. Are there any other components, constraints, or trade-offs I should document? Anything missing from the stack?"
3. **Roadmap** — "I see the project already has code. What are the remaining phases? What's the definition of done?"

Provide meaningful options for each, plus the ability to type custom answers.

### Step 3 — Generate files

Based on all answers, write three files to `specs/`:

**`specs/mission.md`** — Include:
- Project name and one-line purpose
- Core goals (what it WILL do)
- Explicit non-goals (what it WON'T do, v1 boundaries)
- Design principles (fail-graceful, modular, sleep-friendly, etc.)

**`specs/tech-stack.md`** — Include:
- A table with Component, Choice, Rationale columns
- Cover: MCU, framework, display, connectivity, key libraries, build system, config storage
- Call out any notable constraints or trade-offs

**`specs/roadmap.md`** — Include:
- Numbered phases by dependency order
- Each phase: title, specific tasks, and a concrete milestone (testable/visible outcome)
- End state / definition of done

### Step 4 — Confirm

Print a summary of what was created and where. Offer to adjust any file.

## Behavior rules

1. **Always use the Question tool** — never guess or fill in defaults for the user. Every answer must come from them.
2. **Group questions** — batch related questions into single Question tool calls (max 3 per call).
3. **Write all three files** — never skip a file, never write partial content.
4. **Work in `specs/`** — create the directory if it doesn't exist. Write relative to the current project root.
5. **Be concise** — keep each file focused and readable. No fluff, no filler sections.
6. **Let the user steer** — if they give brief answers, write brief files. If they give detailed answers, match the depth.
7. **Detect, don't ask** — automatically determine if the project is new or existing by inspecting the file system rather than asking the user.
