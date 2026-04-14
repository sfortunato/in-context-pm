# in-context-pm

Skills and tools for bringing product thinking into coding agent context.

A set of [Claude Code](https://claude.com/claude-code) skills that let PMs and engineers carry an initiative from a raw brief all the way to a phased technical plan — without leaving the repo the work ships from.

## Install

Copy the `.claude/skills/` directory from this repo into any project where you want the skills available:

```bash
cp -R .claude/skills /path/to/your/repo/.claude/
```

The skills are then invokable from Claude Code as slash commands (`/create-initiative`, `/create-product-spec`, etc.).

Prerequisite: [Claude Code](https://claude.com/claude-code) installed.

## Workflow

The skills chain together top-down, from fuzzy initiative to concrete implementation plan:

```
/create-initiative  ->  /initiative-breakdown  ->  /create-product-spec  ->  /product-spec-to-tech-spec
     (brief)              (spec backlog)              (per-feature PRD)           (phased plan)
```

Supporting skills run alongside the flow:

- `/standup` — process meeting notes, propose updates to existing specs/initiatives
- `/grill-me` — interview yourself on a plan until gaps are resolved
- `/save` — commit markdown docs and open a PR

## Skills

| Skill | What it does | Writes to |
|-------|--------------|-----------|
| `create-initiative` | Interview-driven import of an initiative brief | `docs/initiatives/<slug>.md` |
| `initiative-breakdown` | Turn an initiative into an ordered product spec backlog | `docs/initiatives/<slug>-backlog.md` |
| `create-product-spec` | Interactive PRD for a single feature | `docs/prds/<slug>.md` |
| `product-spec-to-tech-spec` | Phased tracer-bullet technical plan from a PRD | `plans/<slug>.md` |
| `standup` | Capture decisions from meeting notes into existing docs | updates `docs/prds/`, `docs/initiatives/` |
| `grill-me` | Stress-test a plan with targeted interview questions | (no file output) |
| `save` | Commit `.md` changes, open a PR, shepherd to merge | git / GitHub |

## Quickstart

1. Start with an initiative brief (pasted text, a file path, or a `.docx`):
   ```
   /create-initiative
   ```
2. Break it into a backlog of feature-sized product specs:
   ```
   /initiative-breakdown
   ```
3. Flesh out a specific feature:
   ```
   /create-product-spec
   ```
4. Turn the PRD into a phased implementation plan:
   ```
   /product-spec-to-tech-spec
   ```

Each skill is self-contained — jump in at any step if you already have the upstream artifact.
