# LORF Work System — Design

> Evolves the Agent Development Brief into an integrated work management system for LORF projects.

## Problem

No single place to track what needs doing, pick up work, execute it, and record completion into the project stream. Work comes from ad-hoc decisions, mental lists, and scattered notes.

## Solution

A `.lorf/`-native work system with a backlog, structured work directories, and a solutions library for compound knowledge. Managed through `lo:*` skills that connect the full lifecycle from idea to shipped code.

## Directory Structure

```
.lorf/
  BACKLOG.md                          # the queue
  work/
    <feature-name>/
      001-<phase-name>.md             # plan (consumed by executing-plans)
      002-<phase-name>.md
  solutions/                          # compound knowledge
    <topic>.md
  stream/                             # existing — milestone/update entries
  research/                           # existing
  hypotheses/                         # existing
  notes/                              # existing
  project.md                          # existing
```

## File Formats

### BACKLOG.md

```markdown
---
updated: 2026-02-25
---

## Features

### Auth system
User login, signup, password reset. OAuth later.
Status: backlog

### Dashboard redesign
New grid layout with draggable widgets.
Status: active → .lorf/work/dashboard-redesign/

## Tasks
- [ ] Fix button color on settings page
- [ ] Update dependency versions
- [x] ~~Add favicon~~ → 2026-02-20
```

Features get a heading with description and status line. Tasks are checkboxes — checked off inline with a date when done.

**Status values:** `backlog` | `active → .lorf/work/<name>/` | `done → 2026-02-25`

### Work Plans (`.lorf/work/<feature>/001-<phase>.md`)

Same format the executing-plans skill already consumes. Frontmatter with status, plan content, checkpoints. Just a new location.

### Solutions (`.lorf/solutions/<topic>.md`)

```markdown
---
date: 2026-02-25
from: auth-system
tags: [caching, session]
---

## Problem
What we ran into.

## What worked
The approach that solved it.

## Reuse notes
When to apply this again.
```

## Two Lifecycle Paths

### Feature path (heavy)

```
BACKLOG.md feature
  → lo:backlog pick → creates .lorf/work/<name>/
  → brainstorming → design
  → writing-plans → 001-plan.md in .lorf/work/<name>/
  → lo:work → executes plan (branch/worktree, parallel agents, agent teams)
  → lo:ship → test → simplify → security → commit → push → PR
  → stream milestone + solution prompt
```

### Task path (light)

```
BACKLOG.md task
  → just do it
  → check it off
  → batched into next stream update
```

Features graduate into work directories. Tasks never do. Ceremony proportional to complexity.

## Skills

### New skills (lo: namespace)

| Skill | Invocation | Purpose |
|-------|-----------|---------|
| `lo:backlog` | `/backlog` | View backlog, add tasks/features, pick up work |
| `lo:work` | `/work` | Execute plans with branch/worktree, parallel agents, agent teams |
| `lo:solution` | `/solution` | Capture reusable knowledge after completing work |
| `lo:ship` | `/ship` | Test → code-simplifier → security → commit → push → PR |

### Existing skills migrated to lo: namespace

| Current | New |
|---------|-----|
| `lorf-init` | `lo:init` |
| `lorf-milestones` | `lo:milestones` |
| `lorf-hypothesis` | `lo:hypothesis` |
| `lorf-research` | `lo:research` |

### Existing skills updated (stay in superpowers:)

| Skill | Change |
|-------|--------|
| `executing-plans` | Read plans from `.lorf/work/` instead of `.wip/` |
| `finishing-a-development-branch` | Add stream entry creation + solution capture prompt |

## Skill Details

### lo:backlog

**Modes:**
- `/backlog` — display current backlog (features with status, tasks with checkboxes)
- `/backlog task "fix button color"` — add a task checkbox
- `/backlog feature "auth system"` — add a feature heading, prompt for description
- `/backlog pick "auth system"` — create `.lorf/work/auth-system/`, update status to active, bridge to brainstorming

### lo:work

**Behavior:**
1. Check `.lorf/work/` for active feature with plans
2. If no plan exists, bridge to brainstorming → writing-plans first
3. Create branch or worktree (worktree for features, branch for smaller scoped work)
4. Read the current plan and identify parallelizable tasks
5. Execute using the appropriate level of parallelization:
   - Simple sequential work → do it directly
   - Independent tasks → dispatch subagents
   - Large-scope features → use Agent Teams (preview feature enabled)
6. Be transparent about what's running in parallel
7. Stop when plan is complete — does NOT ship (that's `lo:ship`)

### lo:solution

**Behavior:**
1. Prompt: what did you learn, what's reusable, what would future-you want to know?
2. Write to `.lorf/solutions/<topic>.md` with frontmatter (date, source feature, tags)
3. Can be invoked standalone or triggered by `lo:ship`

### lo:ship

**Pipeline:**
1. Run tests
2. Run code-simplifier on changed files
3. Security review (requesting-code-review or dedicated check)
4. Commit with meaningful message
5. Push
6. Create PR via finishing-a-development-branch
7. Create stream milestone entry
8. Prompt: "Anything reusable worth capturing?" → invoke lo:solution if yes

If any gate fails, stop and report what needs fixing.

## Stream Integration

- **Features:** Completing a feature → stream milestone (auto-created by lo:ship)
- **Tasks:** Batched into periodic stream updates ("housekeeping: fixed X, updated Y, added Z")

## Knowledge Compounding

Future brainstorming and planning sessions search `.lorf/solutions/` before starting from scratch. The solutions library grows organically — no upfront structure required, just the prompt at ship time.

## Cross-Project

Per-project `BACKLOG.md`, `.lorf/work/`, and `.lorf/solutions/`. Cross-project view is a future problem — not designed here.

## Migration

- Existing `.wip/` files archived
- Existing ADB docs (`agent-development-brief/`) archived to `/archive`
- `executing-plans` skill updated to read from `.lorf/work/` (with `.wip/` fallback during transition)
