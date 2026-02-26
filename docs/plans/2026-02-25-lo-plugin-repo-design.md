---
title: "LO Plugin — Own Repo"
date: 2026-02-25
status: approved
approach: new-repo
---

# Design: Move lo-plugin to its own repo

## Summary

Create `looselyorganized/lo-plugin` as a standalone repo. Move all 9 skills (8 work system + stocktaper design system), plugin.json, and the `.lo/` convention spec into it. Clean install URL: `github.com/looselyorganized/lo-plugin`.

## Repo structure

```
looselyorganized/lo-plugin/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── backlog/
│   ├── hypothesis/
│   ├── milestones/
│   ├── new/
│   ├── research/
│   ├── ship/
│   ├── solution/
│   ├── stocktaper-design-system/
│   └── work/
├── docs/
│   └── lo-spec.md
├── README.md
├── LICENSE
└── .lo/
    └── (project metadata for the plugin itself)
```

## What moves where

| Source | Destination | Action |
|--------|------------|--------|
| `agent-development-brief/lo-plugin/` (skills, plugin.json, README) | `lo-plugin/` (new repo root) | Move |
| `skills/stocktaper-design-system/` | `lo-plugin/skills/stocktaper-design-system/` | Move |
| `looselyorganized/docs/lo-spec.md` | `lo-plugin/docs/lo-spec.md` | Copy |
| `agent-development-brief/.lo/` | `lo-plugin/.lo/` | Move |

## Updates required

- **plugin.json**: `repository` field → `https://github.com/looselyorganized/lo-plugin`
- **lo-spec.md**: Add `work/`, `solutions/`, `notes/` subdirectories to the directory structure section and add file specification sections for each
- **CLAUDE.md files**: Update any plugin install references across projects to point to new repo

## Post-move cleanup

| Repo | Action |
|------|--------|
| `agent-development-brief` | Keep archive/, docs/plans/, README. Becomes historical brief repo |
| `skills` | Empty after stocktaper moves out. Archive or delete |

## What does NOT change

- Skill file contents (already updated from lorf-to-lo rename)
- `.lo/` convention (directory structure, frontmatter contracts)
- Plugin name (`lo`)
