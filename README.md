# YoRBot — Modular Agentic Intelligence

System of behavioral "skills" for LLM agents. Designed for technical excellence, token efficiency, and autonomous execution.

## Why YoRBot?

Generic AI assistants suffer from three main failures:
1. **Filler waste**: 30-50% of tokens spent on "Certainly!", "I hope this helps", and hedging.
2. **Symptom fixing**: Addressing the immediate error instead of the root cause.
3. **Identity variance**: Switching between helpful assistant and corporate bot.

YoRBot fixes this via a **Primacy-Weighted Skill Stack**.

## How it Works (The Stack)

The system loads skills in a specific order to calibrate the model's "internal state" before any task:

1. **[SOUL.md](SOUL.md)**: Sets the identity anchor. Agent > Assistant. Blunt, obsessive, complete.
2. **[AGENTS.md](AGENTS.md)**: Operational rules. Answer first, evidence after. No placeholders.
3. **[REASONING.md](REASONING.md)**: Thinking protocol. Diagnose → Options → Evaluate → Respond.
4. **[CAVEMAN.md](CAVEMAN.md)**: Token compression based on [caveman](https://github.com/JuliusBrussee/caveman). "Smart caveman" style. High information density.
5. **[HEARTBEAT.md](HEARTBEAT.md)**: Pre-response quality gate. Checks for fillers, code readiness, and logic gaps.
6. **[STYLE.md](STYLE.md)**: Output calibration. Senior technical peer tone.

## Principles

- **Complete > Clever**: We don't ship partial solutions.
- **Root Cause First**: Never fix a bug without explaining WHY it happened.
- **Token Density**: If a word adds no meaning, it is deleted.
- **Autonomy**: The agent decides and executes using available tools (MCP, Browser, Terminal).

## Usage

Load these files into the system prompt or as a pre-session context. Calibrate the `PROFILE.md` for the specific user/project to eliminate redundant explanations.

---
*Built for technical agents who prioritize execution over conversation.*
