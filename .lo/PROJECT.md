---
proj_id: "proj_e62f7efd-d423-4b19-8416-6cb0c9138efa"
title: "Agent Development Brief"
description: "A collaborative framework replacing traditional PRDs for AI-human development teams — minimal upfront spec, living documentation, outcome-focused."
status: "open"
state: "public"
topics:
  - agent-collaboration
  - developer-tooling
  - documentation
repo: "https://github.com/looselyorganized/agent-development-brief"
infrastructure:
  - GitHub
agents:
  - name: "claude-code"
    role: "AI coding agent (Claude Code)"
---

Agent Development Briefs replace traditional Product Requirements Documents with a collaborative framework designed for AI-human development teams. Instead of predicting everything upfront, ADBs define the problem space and let implementation drive specification refinement.

## Capabilities

- **Brief Authoring** — Template-driven core documents with fill-in-the-blank scaffolding
- **Decision Authority Framework** — Standardized authority levels (Human-Decides, Agent-Proposes, Agent-Decides, Collaborative) configurable per decision type
- **Living Documentation** — Documents evolve through implementation; decision-log maintained in real-time by agents
- **Example Projects** — Reference ADB implementation (CatGram) demonstrating the full document set
- **Agent Onboarding Workflow** — Ordered document review sequence culminating in agent-generated implementation plan

## Architecture

Pure documentation framework — no runtime, no build, no dependencies. Markdown templates consumed directly by AI coding agents. Archive holds canonical templates; examples hold filled-out references. Agents read documents in prescribed order, ask clarifying questions, then generate an implementation plan.

## Infrastructure

- **GitHub** — Source hosting, version control, and distribution

## Core Documents

- **main.md** — Orchestrates the review process and sets expectations
- **product-requirements.md** — Problem definition, user stories, success metrics, scope boundaries
- **agent-operating-guidelines.md** — Decision-making framework, development philosophy, authority levels
- **decision-log.md** — Timestamped record of all architectural and product decisions
