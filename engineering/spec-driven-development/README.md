# Spec-Driven Development

A workflow for working with agentic AI, based on the course [Spec-Driven Development with Coding Agents](https://learn.deeplearning.ai/courses/spec-driven-development-with-coding-agents).

This isn't a fancy methodology — it's a practical software engineering practice centered on **requirement specification**. The idea is simple: scrutinize, ask questions, and nail down the requirements before writing code. The work splits into two parts:

- **Constitution** — the project's foundation (mission, tech-stack, roadmap)
- **Feature Spec** — the detailed spec for a specific feature (plan, requirements, validation)

## Roles

- **Constitution** — Defines *what* the project is and *where* it's going. Created once (or revisited when the project pivots). Contains `mission.md`, `tech-stack.md`, and `roadmap.md` under `specs/`.

- **Feature Spec** — Defines *what* to build next and *how* to validate it. Created per feature. Contains `plan.md`, `requirements.md`, and `validation.md` under `specs/`.

## Workflow

1. **Initiate the constitution** → review it yourself. If it's not what you want, keep updating until it is.
2. **Initiate a feature spec** (or another type of spec) → review it yourself. If it's not right, keep updating until it is.
3. **Execute the spec** → review the output. Iterate on this step until the result is what you want, then validate using `validation.md`.
4. **Mark done** — update `validation.md`, `plan.md`, and mark the phase as done in `roadmap.md`.
5. **Repeat** — go back to step 2 for the next spec.
