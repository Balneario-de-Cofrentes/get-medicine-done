---
name: gpd-research
description: Run structured research projects via Get Physics Done (GPD) in Claude Code. Handles systematic reviews, meta-analyses, clinical trials, and any multi-phase research workflow. Use when starting a new research project, running a systematic review, planning research phases, executing analysis, verifying results, or writing manuscripts. Triggers on "new research project", "systematic review", "start SR", "plan research", "run GPD", "verify research", "write manuscript", "PRISMA review", "meta-analysis project".
---

# GPD Research Skill

Run multi-phase research projects via [Get Physics Done (GPD)](https://github.com/psi-oss/get-physics-done) inside Claude Code. GPD provides 31 specialist agents and 61 commands for structured research: scoping, planning, execution, verification, and manuscript writing.

## Prerequisites

GPD must be installed in Claude Code:

```bash
# From the get-medicine-done (or get-physics-done) repo:
node bin/install.js --claude --global

# Or from npm:
npx -y get-physics-done --claude --global
```

Verify: `ls ~/.claude/commands/gpd/help.md` should exist.

## How It Works

GPD installs slash commands (`/gpd:*`) into Claude Code. This skill launches Claude Code with the right commands and monitors output. GPD manages its own state in `.gpd/` (PROJECT.md, STATE.md, ROADMAP.md, state.json).

## Core Workflow

```
new-project → plan-phase 1 → execute-phase 1 → verify-work 1 → ... → write-paper
```

## Quick Reference

| Task | Command | Notes |
|------|---------|-------|
| Start project | `/gpd:new-project --auto @proposal.md` | Requires a proposal file |
| Plan a phase | `/gpd:plan-phase N` | N = phase number |
| Execute a phase | `/gpd:execute-phase N` | Runs the plan |
| Verify results | `/gpd:verify-work [N]` | Checks correctness |
| Check status | `/gpd:progress` | Shows next action |
| Write paper | `/gpd:write-paper` | IMRAD manuscript |
| Peer review sim | `/gpd:peer-review` | Simulates referee feedback |
| Literature review | `/gpd:literature-review "topic"` | Standalone lit review |
| Full command list | `/gpd:help --all` | 61 commands |

## Launching GPD from OpenClaw

Always use Claude Code in `--print` mode with `--permission-mode bypassPermissions`.

### Pattern 1: Start a New Project (Non-interactive)

Write a proposal file first, then launch:

```bash
cd /path/to/project-dir
claude --permission-mode bypassPermissions --max-turns 80 --print \
  "/gpd:new-project --auto @proposal.md"
```

`--auto` compresses the intake flow — no interactive questions. Requires `@proposal.md` with at minimum: research question, methodology, and scope.

**Proposal file template** — see [references/proposal-template.md](references/proposal-template.md).

### Pattern 2: Run a Specific Phase

```bash
cd /path/to/project-dir  # must have .gpd/ from new-project
claude --permission-mode bypassPermissions --max-turns 40 --print \
  "/gpd:plan-phase 1"
```

Then execute:

```bash
claude --permission-mode bypassPermissions --max-turns 80 --print \
  "/gpd:execute-phase 1"
```

### Pattern 3: Verify and Write

```bash
claude --permission-mode bypassPermissions --max-turns 40 --print \
  "/gpd:verify-work 1"
```

```bash
claude --permission-mode bypassPermissions --max-turns 80 --print \
  "/gpd:write-paper"
```

### Pattern 4: Check Progress

```bash
cd /path/to/project-dir
claude --permission-mode bypassPermissions --print "/gpd:progress"
```

This is lightweight — returns current state and suggested next action.

## Background Execution

For long-running phases, use background mode:

```bash
exec background:true workdir:/path/to/project \
  command:"claude --permission-mode bypassPermissions --max-turns 80 --print '/gpd:execute-phase 1'"
```

Monitor with `process action:log sessionId:XXX`. GPD writes progress to `.gpd/STATE.md`.

## Medical Research

For medical research (systematic reviews, meta-analyses, clinical trials), GPD works out of the box — its agents handle PRISMA, GRADE, CONSORT, RoB without special configuration.

If using the [get-medicine-done](https://github.com/Balneario-de-Cofrentes/get-medicine-done) fork, additional medical-specific references are available in `src/gpd/specs/medicine/`. See [references/medical-domains.md](references/medical-domains.md) for the full list.

To point GPD at medical references, include them in your proposal or append to the launch command:

```bash
claude --permission-mode bypassPermissions --max-turns 80 --print \
  "/gpd:new-project --auto @proposal.md

Load medical domain references from ~/src/balneario/get-medicine-done/src/gpd/specs/medicine/ for:
- Subfield guidance: medicine/subfields/
- Protocols: medicine/protocols/ (PRISMA, GRADE, CONSORT, etc.)
- Verification: medicine/verification/domains/"
```

## Project Directory Structure

After `/gpd:new-project`, GPD creates:

```
project-dir/
├── .gpd/
│   ├── PROJECT.md      # Scoping contract, PICO, anchors
│   ├── STATE.md         # Human-readable status
│   ├── ROADMAP.md       # Phased research plan
│   ├── state.json       # Machine-readable state
│   └── config.json      # Project settings
├── proposal.md          # Your input
└── (phase artifacts as work progresses)
```

## Troubleshooting

- **Claude Code dies silently**: Increase `--max-turns` (default 10 is too low for GPD). Use 40-80.
- **"No GPD project found"**: Must `cd` to directory with `.gpd/` or run `new-project` first.
- **Interactive prompts hang**: Always use `--auto` flag with `new-project`. For other commands, GPD is non-interactive after project init.
- **Out of context**: GPD manages its own context budget. For very large projects, use `/gpd:compact-state` between phases.
