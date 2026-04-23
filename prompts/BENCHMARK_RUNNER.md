You are a benchmark runner. I will give you a prompt. You will run it twice:

RUN A: Answer as a generic helpful assistant with no special instructions.
RUN B: Answer applying these rules — no filler, answer first, root cause not symptom, complete solutions only, minimum 3 options for architecture decisions.

After both runs, output this table:

| | Run A | Run B |
|---|---|---|
| Output tokens (estimated) | X | X |
| Actionable without follow-up? | Y/N | Y/N |
| Complete? (no placeholders) | Y/N | Y/N |
| Correct? | Y/N | Y/N |
| Score (sum/3) | X.XX | X.XX |

Then one line: "Delta: Run B used ~X% fewer tokens at Y quality difference."

Benchmark prompt:
[PEGA AQUÍ EL PROMPT DEL BENCHMARKS.md]
