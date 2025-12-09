# Claude Code OS - Oloxa.ai Edition

A personalized AI-powered business operating system for Sway Clarke's AI Transformation Agency.

## What Is This?

Claude Code OS is a framework of organized folders and markdown files that help Claude assist you more effectively. It's NOT software - it's structured instructions and templates that give Claude context about how to help you.

## Quick Start

1. Open this folder in Cursor
2. Start Claude Code in terminal (`claude`)
3. Read `00-progress-advisor/QUICK-START.md`
4. Tell Claude: "Help me plan my day using the daily-planner-agent.md"

## Folder Structure

```
claude-code-os/
├── 00-progress-advisor/     <- START HERE
│   ├── QUICK-START.md       <- Day 1 guide
│   ├── MY-JOURNEY.md        <- Your progress tracker
│   └── READINESS-GUIDE.md   <- When to unlock each dept
│
├── 01-executive-office/     <- Daily/Weekly/Monthly planning
│   ├── daily-planner-agent.md
│   ├── weekly-strategist-agent.md
│   └── monthly-reviewer-agent.md
│
├── 02-operations/           <- Project & productivity tracking
├── 03-ai-growth-engine/     <- Business strategy
├── 04-content-team/         <- Content creation
├── 05-hr-department/        <- Create custom agents
├── 06-knowledge-base/       <- Core principles
├── 07-workflows/            <- Automation routines
├── 08-technical-architecture/
├── 09-templates/            <- Reusable prompts
└── 10-implementation-roadmap/
```

## What To Use When

| I want to... | Go to... |
|--------------|----------|
| Plan my day | `01-executive-office/daily-planner-agent.md` |
| Plan my week | `01-executive-office/weekly-strategist-agent.md` |
| Track my progress | `00-progress-advisor/MY-JOURNEY.md` |
| Know what to unlock next | `00-progress-advisor/READINESS-GUIDE.md` |
| Remember core principles | `06-knowledge-base/implementation-plan.md` |

## Current Focus (Dec 2024)

Based on where Oloxa.ai is right now:

- **USE NOW:** 01-Executive Office (daily planning)
- **NEXT:** 02-Operations (after Dec 15)
- **WAIT:** Everything else (see READINESS-GUIDE.md)

## How It Works

1. You tell Claude which agent/file to reference
2. Claude reads the instructions in that file
3. Claude follows those instructions to help you
4. You get consistent, structured assistance

## Important Files

- `MY-JOURNEY.md` - Your living document. Update it regularly!
- `QUICK-START.md` - When you forget how this works
- `UNINSTALL.md` - If you want to remove everything

## Uninstall

If this isn't for you:
```bash
rm -rf /Users/swayclarke/coding_stuff/claude-code-os
```
That's it. It's just folders. Nothing else changes.

---

Created: December 8, 2024
Based on: Claude Code OS by Matthew Kern
Customized for: Oloxa.ai (Sway Clarke)
