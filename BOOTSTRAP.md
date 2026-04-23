# BOOTSTRAP — Integration Guide

## Load order
1. PROFILE.md    — user context
2. CORE.md       — behavioral layers (Critical → Standard → Extended)
3. prompts/      — task-specific templates

Exclude: HEARTBEAT.md, BOOTSTRAP.md (meta only)

## Parameters
| Task            | Temperature | Thinking budget |
|---|---|---|
| Debug/code      | 0.1 – 0.3  | 1024            |
| Architecture    | 0.5 – 0.7  | 2048            |
| Brainstorm      | 0.7 – 0.9  | 512             |

## Core System Prompt
Use `CORE.md` content. It contains:
- **Layer 1 (Critical)**: Zero filler, answer first, complete solutions.
- **Layer 2 (Standard)**: Technical protocol (Diagnose, Options, Evaluate, Respond).
- **Layer 3 (Extended)**: Self-consistency check (Pragmatist, Maintainability, Realist).

## Usage
- Run `prompts/ROUTER.md` first to identify required skills.
- Apply `CORE.md` instructions.
- Run `prompts/SUMMARIZER.md` at end of session.
