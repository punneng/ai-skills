# ai-skills

A collection of AI agent skill definitions for use with opencode and similar AI coding assistants.

## Language

### Flagged ambiguities

**category / sub-category vs bucket / sub-bucket**:
The `README.md` previously used "category and sub-category". Resolved: use **bucket** and **sub-bucket** throughout.
_Resolution_: Use bucket (top-level group) and sub-bucket (nested group within a bucket).

## Terms

**Skill**:
A self-contained directory with a `SKILL.md` file plus optional resources that teaches an AI agent how to perform a specific task.
_Avoid_: Plugin, tool, module, extension

**Bucket**:
A top-level directory under `skills/` that groups related skills by domain or maturity level (e.g., engineering, deprecated, productivity). Each bucket has a `README.md` that links to every skill's `SKILL.md` with a one-line description.
_Avoid_: Category, folder, group

**Sub-bucket**:
A directory nested inside a bucket that groups related skills within that domain (e.g., `spec-driven-development` under `engineering/`). Sub-buckets have their own `README.md` listing their skills.
_Avoid_: Sub-category, sub-folder (in docs)

**Installation**:
The mechanism by which skills become available to opencode. Currently done by symlinking skill directories into `~/.config/opencode/skills/`. May evolve into package-manager-based distribution.
_Avoid_: Deployment, setup (when referring to making a skill available to the agent)

**Published skill**:
A skill in a bucket that is listed in the top-level `README.md`. Only skills in `engineering/`, `productivity/`, and `misc/` buckets qualify. These are ready for general consumption.
_Avoid_: Public skill, released skill

**Unpublished skill**:
A skill in a bucket that is excluded from the top-level `README.md`. Skills in `personal/`, `in-progress/`, and `deprecated/` buckets are always unpublished. These are private, unfinished, or retired.
_Avoid_: Private skill, hidden skill

## Example dialogue

**Dev**: I wrote a new skill for reviewing PRs. Where should I put it?

**Domain expert**: What bucket does it belong to? If it's for day-to-day code work, put it in `engineering/`. If it's a personal workflow thing, it's `personal/`.

**Dev**: It's definitely for engineering work. But it's still rough — I want to iterate before publishing it.

**Domain expert**: Then it's an **unpublished skill**. Put it in `engineering/` since that's its permanent home, but don't add it to the top-level `README.md` yet. Actually, better to use the `in-progress/` bucket — that's specifically for unfinished skills and is automatically excluded from the README.

**Dev**: And when it's ready, I move it to `engineering/` and add it to the top-level README?

**Domain expert**: Exactly. At that point it becomes a **published skill**. Make sure the `engineering/` bucket's README already has it linked to its `SKILL.md`. Then add the entry to the top-level `README.md`.

**Dev**: Got it. And installation — I just symlink the skill directory into `~/.config/opencode/skills/`, right?

**Domain expert**: For now, yes. Long-term we might use a package manager, but symlinking keeps it simple while we're building the collection.
