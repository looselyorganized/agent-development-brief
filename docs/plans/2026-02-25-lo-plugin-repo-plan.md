# LO Plugin Repo Migration — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Move the lo-plugin out of agent-development-brief into its own repo at `looselyorganized/lo-plugin`.

**Architecture:** Create a new GitHub repo, copy files from two source repos (agent-development-brief and skills), update the lo-spec, update plugin.json, and clean up references. No code changes — purely file moves, content updates, and git operations.

**Tech Stack:** git, gh CLI

---

### Task 1: Create the new repo on GitHub

**Step 1: Create the repo**

```bash
gh repo create looselyorganized/lo-plugin --public --description "LO work system plugin for Claude Code — backlog, work execution, knowledge capture, and shipping pipeline" --license MIT
```

**Step 2: Clone it locally**

```bash
cd /Users/bigviking/Documents/github/projects/looselyorganized
git clone https://github.com/looselyorganized/lo-plugin.git
cd lo-plugin
```

**Step 3: Verify**

```bash
pwd
# Expected: /Users/bigviking/Documents/github/projects/looselyorganized/lo-plugin
ls
# Expected: LICENSE README.md
```

**Step 4: Commit**

No commit needed — repo already initialized by GitHub.

---

### Task 2: Move plugin structure into new repo

**Files:**
- Copy from: `agent-development-brief/lo-plugin/.claude-plugin/` → `lo-plugin/.claude-plugin/`
- Copy from: `agent-development-brief/lo-plugin/skills/` → `lo-plugin/skills/`
- Copy from: `agent-development-brief/lo-plugin/README.md` → `lo-plugin/README.md`

**Step 1: Copy the plugin skeleton**

```bash
cd /Users/bigviking/Documents/github/projects/looselyorganized/lo-plugin
cp -r ../agent-development-brief/lo-plugin/.claude-plugin .
cp -r ../agent-development-brief/lo-plugin/skills .
cp ../agent-development-brief/lo-plugin/README.md .
```

**Step 2: Verify structure**

```bash
ls -la .claude-plugin/
# Expected: plugin.json
ls skills/
# Expected: backlog hypothesis milestones new research ship solution work
```

**Step 3: Commit**

```bash
git add .claude-plugin/ skills/ README.md
git commit -m "feat: add plugin structure with 8 work system skills"
```

---

### Task 3: Add stocktaper design system skill

**Files:**
- Copy from: `skills/stocktaper-design-system/` → `lo-plugin/skills/stocktaper-design-system/`

**Step 1: Copy the skill**

```bash
cd /Users/bigviking/Documents/github/projects/looselyorganized/lo-plugin
cp -r ../skills/stocktaper-design-system skills/
```

**Step 2: Verify**

```bash
ls skills/stocktaper-design-system/
# Expected: SKILL.md (and any reference files)
```

**Step 3: Commit**

```bash
git add skills/stocktaper-design-system/
git commit -m "feat: add stocktaper design system skill"
```

---

### Task 4: Copy and update the lo-spec

**Files:**
- Copy from: `looselyorganized/docs/lo-spec.md` → `lo-plugin/docs/lo-spec.md`
- Modify: `lo-plugin/docs/lo-spec.md` — add `work/`, `solutions/`, `notes/` subdirectories

**Step 1: Copy the spec**

```bash
cd /Users/bigviking/Documents/github/projects/looselyorganized/lo-plugin
mkdir -p docs
cp ../looselyorganized/docs/lo-spec.md docs/
```

**Step 2: Update the directory structure section**

In `docs/lo-spec.md`, find the directory structure block (around lines 17-30) and replace it with:

```
.lo/
├── PROJECT.md            # Brief, metadata, agent declarations
├── BACKLOG.md            # Feature and task backlog
├── hypotheses/           # One file per hypothesis
│   ├── h001-redis-locking.md
│   └── h002-crdt-state.md
├── stream/               # Milestones, updates, notes
│   ├── 2026-01-15-project-started.md
│   ├── 2026-02-15-prototype-deployed.md
│   └── 2026-02-17-load-test-results.md
├── research/             # Research docs (raw/drafts)
│   ├── distributed-locking.md
│   └── institutional-memory.md
├── work/                 # Active feature work directories
│   └── feature-name/
│       └── plan.md
├── solutions/            # Reusable knowledge captured after shipping
│   └── topic-slug.md
└── notes/                # Informal scratch notes
    └── observation.md
```

**Step 3: Add file specification sections for the new subdirectories**

After the `research/*.md` section (around line 170), add these sections:

```markdown
---

### `work/` — Active Feature Work

Contains directories for features graduated from the backlog. Each feature gets its own directory with plan files.

**Directory convention:** `work/<feature-slug>/` (e.g., `work/changing-lorf-to-lo/`)

Plans follow the numbered convention: `plan.md` for single-phase features, or `001-phase-name.md`, `002-phase-name.md` for multi-phase work.

Managed by `/lo:backlog pick` (creates directory) and `/lo:work` (executes plans).

---

### `solutions/` — Reusable Knowledge

Captures reusable patterns and knowledge after completing work. Solutions compound over time — future brainstorming and planning sessions search this directory before starting from scratch.

**Filename convention:** `<topic-slug>.md` (e.g., `parallel-agent-dispatch.md`)

**Frontmatter contract:**

```yaml
---
title: "Parallel Agent Dispatch"
date: "2026-02-25"
feature: "changing-lorf-to-lo"
tags:
  - subagents
  - parallelization
---
```

**Body:** What was learned, what's reusable, concrete patterns. Free-form Markdown.

**Required fields:** `title`, `date`
**Optional fields:** `feature`, `tags`

Managed by `/lo:solution`.

---

### `notes/` — Scratch Notes

Informal observations, thoughts, and reference material that don't fit elsewhere. Not synced to the website.

**Filename convention:** `<slug>.md` — no date prefix required.

No frontmatter contract — freeform.
```

**Step 4: Update the version**

Change `> Version 0.1.0 — 2026-02-18` to `> Version 0.2.0 — 2026-02-25`

**Step 5: Commit**

```bash
git add docs/
git commit -m "docs: add lo-spec with work, solutions, and notes subdirectories"
```

---

### Task 5: Move .lo/ project metadata

**Files:**
- Copy from: `agent-development-brief/.lo/` → `lo-plugin/.lo/`

**Step 1: Copy the metadata**

```bash
cd /Users/bigviking/Documents/github/projects/looselyorganized/lo-plugin
cp -r ../agent-development-brief/.lo .
```

**Step 2: Verify**

```bash
ls .lo/
# Expected: BACKLOG.md PROJECT.md hypotheses notes research solutions stream work
```

**Step 3: Commit**

```bash
git add .lo/
git commit -m "feat: add .lo/ project metadata"
```

---

### Task 6: Update plugin.json

**Files:**
- Modify: `lo-plugin/.claude-plugin/plugin.json`

**Step 1: Update repository URL and add stocktaper to keywords**

Change `plugin.json` to:

```json
{
  "name": "lo",
  "description": "LO work system — backlog, work execution, knowledge capture, shipping pipeline, and design system for Loosely Organized projects",
  "version": "1.1.0",
  "author": {
    "name": "Loosely Organized Research Facility"
  },
  "repository": "https://github.com/looselyorganized/lo-plugin",
  "license": "MIT",
  "keywords": ["lo", "work-management", "backlog", "skills", "design-system"]
}
```

**Step 2: Commit**

```bash
git add .claude-plugin/plugin.json
git commit -m "chore: update repo URL and add design-system keyword"
```

---

### Task 7: Update README

**Files:**
- Modify: `lo-plugin/README.md`

**Step 1: Update the README**

The current README references `agent-development-brief`. Rewrite it to be a proper plugin README with install instructions, skill list, and a link to the lo-spec. Include stocktaper in the skill table.

At minimum, add an install section:

```markdown
## Install

Add to your Claude Code settings:

```json
{
  "plugins": ["github.com/looselyorganized/lo-plugin"]
}
```

And add stocktaper to the skills table.

**Step 2: Commit**

```bash
git add README.md
git commit -m "docs: update README with install instructions and full skill list"
```

---

### Task 8: Push and verify

**Step 1: Push to GitHub**

```bash
cd /Users/bigviking/Documents/github/projects/looselyorganized/lo-plugin
git push
```

**Step 2: Verify repo on GitHub**

```bash
gh repo view looselyorganized/lo-plugin
```

Expected: Repo exists, description matches, files visible.

---

### Task 9: Clean up source repos

**Step 1: Remove lo-plugin from agent-development-brief**

```bash
cd /Users/bigviking/Documents/github/projects/looselyorganized/agent-development-brief
rm -rf lo-plugin/
git add -A
git commit -m "chore: remove lo-plugin (moved to looselyorganized/lo-plugin)"
git push
```

**Step 2: Archive the skills repo (optional — confirm with user)**

The `skills/` repo only has `stocktaper-design-system/`, `assets/`, and a README after the move. Ask user whether to archive it.

---

### Task 10: Initialize .lo/ in the new plugin repo

Since the .lo/ was copied from agent-development-brief, the PROJECT.md may reference the old repo. Read it and update any references to point to the new `lo-plugin` repo. Also run `/lo:new` equivalent checks to make sure the .lo/ structure is valid for the new repo context.

**Step 1: Read and update PROJECT.md**

Update the `repo` field in `.lo/PROJECT.md` frontmatter to `https://github.com/looselyorganized/lo-plugin`.

**Step 2: Commit and push**

```bash
git add .lo/PROJECT.md
git commit -m "chore: update project metadata for new repo"
git push
```
