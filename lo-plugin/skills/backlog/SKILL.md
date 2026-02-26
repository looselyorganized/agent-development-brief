---
name: backlog
description: Manages the LORF project backlog in .lorf/BACKLOG.md. Supports viewing the backlog, adding tasks, adding features, and picking features to graduate into .lorf/work/ directories. Use when user says "backlog", "add task", "add feature", "what should I work on", "what's next", "start a feature", "/backlog", "/task", or "/feature".
---

# LORF Backlog Manager

Manages the project backlog at `.lorf/BACKLOG.md`. Features and tasks live here until they graduate into active work.

## When to Use

- User invokes `/lo:backlog`
- User says "add task", "add feature", "what's next", "what should I work on"
- User wants to pick up a feature and start working on it

## Critical Rules

- `.lorf/` directory MUST exist. If it doesn't, tell the user to run `/lo:new` first.
- If `.lorf/BACKLOG.md` doesn't exist, create it with the default template before proceeding.
- ALWAYS read the current backlog before making changes — never overwrite blindly.
- Update the `updated:` date in frontmatter whenever the file is modified.
- Feature status values: `backlog` | `active -> .lorf/work/<name>/` | `done -> YYYY-MM-DD`
- Tasks are checkboxes: `- [ ]` open, `- [x] ~~text~~ -> YYYY-MM-DD` done.
- All files are plain Markdown with YAML frontmatter. No MDX.

## Modes

Detect mode from arguments. `/lo:backlog` with no args → view. `/lo:backlog task "fix X"` → add task. `/lo:backlog feature "auth"` → add feature. `/lo:backlog start "auth"` → start feature.

### Mode 1: View Backlog

No arguments. Read `.lorf/BACKLOG.md` and display a summary:

    Backlog (updated YYYY-MM-DD):

    Features:
      [backlog] Feature Name — short description
      [active]  Feature Name -> .lorf/work/feature-name/
      [done]    Feature Name -> completed YYYY-MM-DD

    Tasks:
      [ ] Task description
      [x] Completed task -> YYYY-MM-DD

If backlog is empty, suggest adding items.

### Mode 2: Add Task

Arguments: `task "description"`

1. Read current BACKLOG.md
2. Append a new checkbox under `## Tasks`: `- [ ] description`
3. Update `updated:` date
4. Confirm: `Task added: "description"`

If no description provided, prompt for one.

### Mode 3: Add Feature

Arguments: `feature "name"`

1. Read current BACKLOG.md
2. Ask for a 1-2 sentence description if not provided
3. Append under `## Features`:

        ### Feature Name
        Description of the feature.
        Status: backlog

4. Update `updated:` date
5. Confirm: `Feature added: "Feature Name" (status: backlog)`

### Mode 4: Start Feature

Arguments: `start "name"`

Graduates a feature from backlog to active work.

1. Read current BACKLOG.md
2. Find the matching feature (fuzzy match on name)
3. If not found, show available backlog features and ask user to choose
4. Derive directory name: kebab-case from feature name
5. Create `.lorf/work/<feature-name>/` directory
6. Update the feature's status line: `Status: active -> .lorf/work/<feature-name>/`
7. Update `updated:` date
8. Confirm and bridge:

        Feature started: "<name>"
        Work directory: .lorf/work/<feature-name>/

        No plans exist yet. Ready to brainstorm the design?

### Mode 5: Complete Task

When user says "done with X", "finished X", or checks off a task:

1. Find the matching task in BACKLOG.md
2. Update it: `- [x] ~~description~~ -> YYYY-MM-DD`
3. Update `updated:` date

### Mode 6: Complete Feature

When a feature is shipped (typically triggered by lo:ship, not invoked directly):

1. Update the feature's status: `Status: done -> YYYY-MM-DD`
2. Update `updated:` date

## Default BACKLOG.md Template

If BACKLOG.md doesn't exist, create it:

    ---
    updated: YYYY-MM-DD
    ---

    ## Features

    _No features yet. Use `/lo:backlog feature "name"` to add one._

    ## Tasks

    - [ ] Review project.md and fill any TODO placeholders

Use today's date for the `updated` field.
