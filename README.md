# ai-skills

My collection of AI skills for use with [opencode](https://opencode.ai) and similar AI coding assistants. Currently focused on **software engineering**, with more skills to come across various domains.

## Structure

Skills are organized into buckets and sub-buckets:

```
skills/
├── engineering/                       # bucket
│   └── spec-driven-development/       # sub-bucket
│       ├── constitute/                # Create project constitution
│       └── specify-feature/           # Create feature specs
├── in-progress/                       # bucket (unpublished)
│   └── notion/                        # Notion page/database operations
└── ...more buckets coming
```

Each skill lives in its own directory with a `SKILL.md` file that defines its instructions and workflows.

## Installation

Skills are loaded from `~/.config/opencode/skills/`. To install a skill, symlink it to the corresponding path:

```bash
# Example: install all engineering skills
ln -s "$(pwd)/skills/engineering" ~/.config/opencode/skills/engineering

# Or install a specific skill
ln -s "$(pwd)/skills/engineering/spec-driven-development/constitute" ~/.config/opencode/skills/engineering/spec-driven-development/constitute
```

## Skills

### Engineering

<!-- TODO: add README for engineering/ -->

- **spec-driven-development/constitute** — Create a project constitution (mission.md, tech-stack.md, roadmap.md) in `specs/` using structured Q&A.
- **spec-driven-development/specify-feature** — Create a feature spec (plan.md, requirements.md, validation.md) under `specs/` for the next unstarted roadmap phase.
