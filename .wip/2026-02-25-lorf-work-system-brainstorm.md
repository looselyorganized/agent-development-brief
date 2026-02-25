# Evolving ADB into LORF's Work System

> **For Claude:** This is a brainstorming session. Read this document fully before responding. Continue from where we left off — we're in the brainstorming skill, past the research phase, ready to explore approaches.

## Context

We're designing a work management system for LORF (Loosely Organized Research Facility). The system evolves the Agent Development Brief (ADB) — written by this project's author a year ago — into a mature, integrated work system that connects to the existing LORF ecosystem.

## The Problem

Today there's no single place to:
1. Track what work needs doing (backlog)
2. Pick up a piece of work and execute it
3. Automatically record completion into the project stream

Work comes from a mix of ad-hoc decisions, mental lists, and scattered notes. The LORF stream (`.lorf/stream/`) exists but requires manual `/lorf-milestones` invocations to catch up — it paraphrases git history rather than recording work completion events.

## What We Already Have

### LORF Ecosystem (per-project)
- `.lorf/stream/` — milestone/update/note entries with `commits:` counts
- `.lorf/hypotheses/` — testable hypotheses
- `.lorf/research/` — structured research articles
- `.lorf/notes/` — informal scratch
- `.lorf/project.md` — project identity and metadata
- `.wip/` — design docs and implementation plans (consumed by executing-plans skill)

### Existing Skills
- `brainstorming` — idea → design through collaborative dialogue
- `writing-plans` — design → implementation plan
- `executing-plans` — plan → code (batch execution with checkpoints)
- `finishing-a-development-branch` — merge/PR/cleanup
- `lorf-milestones` — scan git history and generate stream entries

### ADB (the starting point to evolve)
- `main.md` — entry point / orchestrator
- `product-requirements.md` — problem, users, scope, success metrics
- `agent-operating-guidelines.md` — decision authority matrix
- `decision-log.md` — running audit trail of decisions
- `technical-guidelines.md` — stack, constraints, standards
- `design-requirements.md` — UX goals, flows, visual constraints
- `PROJECT_STATUS.md` — agent-generated phased implementation plan

## Research: What Others Built

### GSD (Get Shit Done) — 20k stars
- `.planning/` directory with phases, plans, summaries
- Plans are prompts (YAML frontmatter + XML tasks)
- Phase cycle: discuss → plan → execute → verify
- Multi-agent orchestration, wave-based parallelization
- `ROADMAP.md` as the backlog, `STATE.md` as cross-session memory
- **Takeaway:** Phase structure is solid, but the system is heavy — plans-as-prompts with XML tasks is overengineered for a small team

### Compound Engineering — 9.5k stars
- `docs/plans/`, `docs/brainstorms/`, `docs/solutions/`
- Brainstorm → plan → work → review → compound cycle
- **The killer idea:** `docs/solutions/` captures every solved problem, and future planning searches it. Knowledge compounds over time.
- 15 parallel review agents
- **Takeaway:** The compound knowledge loop is the best idea in the space. `.lorf/` already has the directory structure for this but no habit of capturing "here's what I learned"

### Spec Kit (GitHub) — 72k stars
- `.specify/` directory with numbered feature specs
- Spec → plan → tasks → implement workflow
- Constitution.md as immutable project rules
- `[P]` parallelization markers in task lists
- Clean separation: spec.md (what/why) vs plan.md (how) vs tasks.md (do)
- **Takeaway:** Numbered feature directories and the spec/plan/tasks separation are clean patterns. The constitution concept maps to LORF's `project.md`

## ADB's Advantages (Prior Art)

ADB anticipated ideas that GSD, Compound, and Spec Kit all landed on independently:
- **Living docs over upfront specs** — "embrace that most upfront specs are wrong"
- **Decision authority matrix** — `[Agent-Decides]` / `[Human-Decides]` / `[Collaborative]` (predates GSD's auto/checkpoint task types)
- **Phased execution with testability** — `PROJECT_STATUS.md` with pass/fail tests per phase
- **Agent-maintained decision log** — `decision-log.md` (predates Compound's `docs/solutions/`)

## What ADB Is Missing

1. **No backlog** — nowhere to say "here are the things I want to build next"
2. **No stream integration** — `PROJECT_STATUS.md` dies when the project is done; nothing flows into a persistent record
3. **No knowledge compounding** — `decision-log.md` captures decisions but not reusable solutions
4. **No connection to the skill system** — brainstorming/planning/executing skills exist but ADB doesn't invoke them
5. **Single-project scoped** — ADB lives inside one project; LORF spans multiple projects under one org

## Design Questions Still Open

1. **What's the unit of work?** Feature? Task? Something else? How granular?
2. **Where does the backlog live?** A single `BACKLOG.md`? A `work/` directory with one file per item? Numbered directories like Spec Kit?
3. **What's the lifecycle?** idea → planned → in-progress → done → streamed? Simpler?
4. **How does "done" trigger a stream entry?** Automatically from a skill? From the commit message? From closing the work item?
5. **Does the decision log evolve into a solutions library?** Should `.lorf/solutions/` exist alongside `.lorf/research/`?
6. **How does this work across 5 LORF projects?** Shared backlog? Per-project backlogs? Both?

## Where We Left Off

We agreed to evolve ADB into LORF's work system. The next step in the brainstorming process is to explore 2-3 approaches with trade-offs and pick a direction.
