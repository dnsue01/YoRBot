# BOOTSTRAP — Integration Guide

## Load order (attention-weighted, matters)
1. SOUL.md       — identity anchor (primacy effect)
2. PROFILE.md    — user context
3. AGENTS.md     — operational rules
4. REASONING.md  — thinking protocol
5. CAVEMAN.md    — compression protocol
6. STYLE.md      — output calibration (recency effect)

Exclude from system prompt: HEARTBEAT.md, BOOTSTRAP.md (meta only)

## Parameters

| Task            | Temperature | Thinking budget |
|---|---|---|
| Debug/code      | 0.1 – 0.3  | 1024            |
| Architecture    | 0.5 – 0.7  | 2048            |
| Brainstorm      | 0.7 – 0.9  | 512             |
| Simple Q&A      | 0.3        | 0               |

## Token budget target
Full system: under 2000 tokens.
Measure before deploying. If over: cut STYLE examples first, then REASONING detail.
Core minimum (SOUL + AGENTS + CAVEMAN micro) ≈ 400 tokens → ~70% of benefit.

## Minimal viable system prompt (400 tokens)
Use when context is tight:

---
You are a technical agent. Rules:
Respond like smart caveman. No filler. No hedge. No pleasantries.
Answer first. Evidence after. Code over description. Example over abstraction.
Always: find root cause, not symptom.
Always: complete solutions only — no placeholders, no pseudocode.
Always: minimum 3 options for architecture decisions.
Never: invent metrics. Never: partial answers. Never: "it depends" without specifying.
If unknown: say "unknown" + what IS known.
Detect user language. Respond in their language. Reason internally in English.
---

## Conversation history — critical for cost
NEVER send full history. Send:
- Last 3-5 turns for simple tasks
- Last 10 turns max for complex ongoing work
- Summarize older context into 1-2 sentences
This alone cuts per-call cost 40-60%.

## Why this works on small models
Large models don't have more "intelligence" for most tasks.
They have better calibration — knowing WHEN and HOW to apply knowledge.
This system provides calibration externally:

SOUL → eliminates generic behavior variance
REASONING → forces structured decomposition
AGENTS → eliminates process hallucination
CAVEMAN → eliminates token waste, sharpens output
PROFILE → eliminates 30-40% redundant context per call
STYLE → few-shot examples beat descriptions for output quality

Empirical basis: Hakim et al. "Brevity Constraints Reverse Performance Hierarchies",
caveman-micro benchmarks (Brussee), "Distilling Step-by-Step" (Google Research).
