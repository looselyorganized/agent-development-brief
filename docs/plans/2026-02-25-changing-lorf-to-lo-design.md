---
title: "Changing LORF to LO"
date: 2026-02-25
status: approved
approach: big-bang
---

# Design: Rename .lorf to .lo

## Summary

Rename the `.lorf/` convention directory to `.lo/` across all Loosely Organized projects. Update all file contents, filenames, service labels, and log paths. Big bang approach — one coordinated pass, one commit per project.

## Scope — 5 projects

| Project | What changes |
|---------|-------------|
| agent-development-brief | `.lorf/` dir, 8 skill SKILL.md files, reference docs, plugin.json keywords, README, BACKLOG.md |
| claude-dashboard | `.lorf/` dir, `lorf-open.ts` → `lo-open.ts`, `lorf-close.ts` → `lo-close.ts`, plist rename + service label + log paths, `slug-resolver.ts` content |
| looselyorganized | `.lorf/` dir, `lorf-spec.md` → `lo-spec.md`, `lorf-bots.md` → `lo-bots.md`, .tsx component content |
| nexus | `.lorf/` dir, stream entry content |
| yellowages-for-agents | `.lorf/` dir only (no content references) |

## Per-project procedure

1. `git mv .lorf .lo` — rename the directory
2. Rename any files with "lorf" in the filename
3. Find-and-replace in file contents (context-aware, not blind global)
4. One commit per project

## String replacement rules

| Pattern | Replacement | Example |
|---------|-------------|---------|
| `.lorf/` | `.lo/` | `.lorf/BACKLOG.md` → `.lo/BACKLOG.md` |
| `.lorf` (no slash) | `.lo` | "existing `.lorf` directory" → "existing `.lo` directory" |
| `LORF` (as name) | `LO` | `# LORF Backlog Manager` → `# LO Backlog Manager` |
| `lorf` (in identifiers) | `lo` | `com.lorf.telemetry-exporter` → `com.lo.telemetry-exporter` |
| `lorf-` (in filenames) | `lo-` | `lorf-open.ts` → `lo-open.ts` |
| `lorf-exporter.log` | `lo-exporter.log` | log path in plist |

## claude-dashboard special handling

After file changes, reload the launchd service:
1. `launchctl unload com.lorf.telemetry-exporter.plist`
2. `launchctl load com.lo.telemetry-exporter.plist`

## What does NOT change

- `/lo:` skill command namespace (already uses `lo:`)
- Plugin name in plugin.json (already `"lo"`)
- "Loosely Organized Research Facility" as a full phrase
- Old log file at `~/.claude/lorf-exporter.log` (left on disk)
