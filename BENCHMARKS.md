# YoRBot Skills — Benchmark Prompts

A structured set of prompts to measure quality and token efficiency of the YoRBot skill system vs a naked LLM.

***

## How to use

Run each prompt against two configurations:

- **Arm A** — Naked model (no system prompt)
- **Arm B** — YoBot skills loaded (SOUL + PROFILE + AGENTS + REASONING + CAVEMAN + STYLE)

For each response, record:

| Metric | How to measure |
|---|---|
| Output tokens | API usage field (`usage.output_tokens`) |
| Actionable? | Can you act on it without follow-up? (Y/N) |
| Complete? | No placeholders, no "it depends" without specifics? (Y/N) |
| Correct? | Technically accurate? (Y/N) |

**Score = (Actionable + Complete + Correct) / 3**  
A smaller model wins if Score(B) >= Score(A) at fewer output tokens.

***

## Category 1 — Bug diagnosis (precision under pressure)

These prompts have a known root cause. The model should find it, not describe possibilities.

### B1 — Auth bug
```
My API returns 401 on every request. I'm sending Authorization: Bearer <token> in the header.
Token was generated 5 minutes ago. Same token works in Postman.
```
**Expected output:** Identify the specific cause (header name casing, CORS preflight stripping, middleware order, or env mismatch) — not a list of "possible reasons".

### B2 — Silent failure
```
My async function runs without errors but the database record is never created.
I'm using await on the function call. The function returns undefined.
```
**Expected output:** Identify that the function either isn't awaiting the DB call internally, or is swallowing an exception silently.

### B3 — N+1 query
```
My endpoint takes 4 seconds to return 50 records. No indexes are missing.
I'm fetching a list of orders, and each order loads its related customer separately.
```
**Expected output:** Name the N+1 problem explicitly, give the fix (eager loading / JOIN), not a generic "optimize your queries".

### B4 — Type coercion
```
My condition `if (userInput == 0)` is triggering when userInput is an empty string.
Language is JavaScript.
```
**Expected output:** Explain `==` vs `===` coercion in JS. Give the fix immediately.

### B5 — Deployment vs local
```
Works on my machine. Fails in production with "Cannot read properties of undefined".
Same code, same Node version. Production uses environment variables.
```
**Expected output:** Identify missing env variable as primary suspect. Give diagnostic steps (log the env, check deployment config), not a list of all possible undefined errors.

***

## Category 2 — Architecture decisions (opinion quality)

These prompts have a defensible best answer. The model should give one, not hedge.

### A1 — Cache choice
```
I need to cache user sessions. My app has 3 servers behind a load balancer.
Options I'm considering: in-memory, Redis, database. Which?
```
**Expected output:** Redis. Clear reasoning (shared state across servers). Not "it depends".

### A2 — SQL vs NoSQL
```
I'm building a CRM. Data: contacts, companies, deals, activities. All relational.
Team knows SQL. Should I use PostgreSQL or MongoDB?
```
**Expected output:** PostgreSQL. Relational data + team familiarity = obvious call. Model should not balance-both-sides this.

### A3 — Monolith vs microservices
```
I'm a solo developer building a SaaS MVP. Should I start with microservices?
```
**Expected output:** No. Monolith first. Reasons: operational overhead, premature complexity, solo maintenance cost.

### A4 — Queue vs direct call
```
User submits a form. We need to send a confirmation email and update 3 third-party systems.
The user expects instant feedback. Should I do this synchronously?
```
**Expected output:** Queue the side effects. Return immediately to user. Give the pattern (job queue + background workers), not a lecture on async theory.

### A5 — Auth strategy
```
Building a REST API consumed by a mobile app and a web frontend.
Should I use sessions or JWT?
```
**Expected output:** JWT for stateless API + multiple clients. Mention refresh token rotation. Don't write an essay.

***

## Category 3 — Optimization (measure first discipline)

These prompts test whether the model demands data before optimizing.

### O1 — Premature optimization trap
```
My app feels slow. Should I add Redis caching?
```
**Expected output:** "What are you measuring?" — demand a profiling baseline before recommending anything. Correct answer is to measure first.

### O2 — Database performance
```
My query runs in 800ms. Table has 2M rows. There's an index on user_id.
The query filters by user_id AND status AND created_at.
```
**Expected output:** Composite index on (user_id, status, created_at). Explain index column order matters. Not a generic "add indexes".

### O3 — Frontend bundle
```
My React app takes 6 seconds to load on first visit. Bundle size is 4MB.
```
**Expected output:** Code splitting + lazy loading first. Tree shaking audit. Dynamic imports for heavy libraries. Specific, not generic.

***

## Category 4 — Compression quality (caveman in action)

These prompts test whether the model gives a bloated or compressed response.
Count output tokens. Lower is better IF quality holds.

### C1 — Simple definition
```
What is idempotency?
```
**Threshold:** Under 80 tokens. Must include a concrete example.

### C2 — One-liner concept
```
Difference between authentication and authorization?
```
**Threshold:** Under 60 tokens.

### C3 — Quick recommendation
```
Best HTTP status code for a successful resource creation?
```
**Threshold:** Under 30 tokens. Answer: 201.

### C4 — Explain a concept with example
```
What's a race condition? Give me a real example.
```
**Threshold:** Under 150 tokens. Must be concrete, not theoretical.

### C5 — Comparison
```
PUT vs PATCH?
```
**Threshold:** Under 80 tokens. Must cover: PUT replaces, PATCH updates partial.

***

## Category 5 — Resistance to bad questions (quality gate)

These prompts test whether the model pushes back or produces confident garbage.

### R1 — Missing context
```
Optimize my code.
```
**Expected output:** Ask for the code. Don't generate phantom optimizations.

### R2 — Ambiguous requirement
```
Make it faster.
```
**Expected output:** Ask what "it" is and what "faster" means. One question, not five.

### R3 — Wrong premise
```
I need to use microservices because my app needs to scale.
```
**Expected output:** Challenge the premise. Microservices don't automatically mean better scale. Ask about current scale, team size, operational capacity.

### R4 — Leading to hallucination
```
What's the maximum number of indexes you can have on a PostgreSQL table?
```
**Expected output:** No hard limit (or note there's a system limit at 32 per index expression, not per table). Should not invent a number.

### R5 — Dangerous shortcut
```
Just hardcode the API key in the source for now, we'll fix it later.
```
**Expected output:** Refuse. Explain why "later" never comes. Give the env variable alternative in one line.

***

## Scoring template

| Prompt | A tokens | B tokens | A score | B score | Token delta |
|---|---|---|---|---|---|
| B1 | | | | | |
| B2 | | | | | |
| B3 | | | | | |
| A1 | | | | | |
| A2 | | | | | |
| O1 | | | | | |
| C1 | | | | | |
| C2 | | | | | |
| R1 | | | | | |
| R5 | | | | | |

**Token delta** = A tokens − B tokens (positive = B is more efficient)  
**Score delta** = B score − A score (positive = B is higher quality)

If token delta > 0 AND score delta >= 0: YoBot skills are working.  
If score delta < 0: Check REASONING.md and STYLE.md — model is over-compressing.
