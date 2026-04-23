# YoBot Skills
> why use big model when small model + right prompt do trick

YoBot is a modular behavioral system for LLM agents. It eliminates token waste and ensures technical precision through structured protocols.

## Before / After
| Input | Naked Model | YoBot Skills |
|---|---|---|
| "My code is slow." | "There are many reasons... (long essay)" | "Run EXPLAIN ANALYZE. Check N+1 queries." |
| "Add Redis?" | "Redis is great! It depends on..." | "What are you measuring? Profile first." |
| "Auth bug 401." | "401 means Unauthorized. Check key..." | "Token expired? Check Bearer header format." |

## What's inside
- **CORE_SKILLS.md**: Consolidated behavioral layers (Critical, Standard, Extended).
- **ROUTER.md**: Dynamic query classifier for skill selection.
- **BENCHMARKS.md**: Structured quality and efficiency tests.
- **PROFILE.md**: Session-invariant user context.
- **SUMMARIZER.md**: Automatic profile updates from session history.

## Benchmark results
| Prompt | Naked tokens | Skills tokens | Quality delta |
|---|---|---|---|
| Bug Diagnosis | [TBD] | [TBD] | [TBD — run benchmarks] |
| Architecture | [TBD] | [TBD] | [TBD — run benchmarks] |
| Optimization | [TBD] | [TBD] | [TBD — run benchmarks] |

## Install
1. Clone repository.
2. Load `prompts/CORE_SKILLS.md` into system prompt.
3. Calibrate `PROFILE.md` with user context.
4. Run `BENCHMARKS.md` to verify performance.

## Why it works
- **Primacy-Weighted Layers**: Forces model state calibration before task execution.
- **Caveman Protocol**: Permission-based brevity significantly reduces token waste.
- **External Reasoning**: Enforces structured diagnosis, reducing logic hallucinations.

## License
MIT
