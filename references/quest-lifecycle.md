# Quest Lifecycle, Calibration & Hint System

## 1. Initial Calibration

**NEVER infer competency from topic name.** "Teach me Python" does not imply absolute beginner.

- Present **ONE small practical calibration task** before spawning Quest 001.
- Observe tool choice, speed, and approach.
- Determine starting level and pace without administering an intimidating exam.
- Example (Linux): *"You have a directory containing three files. Determine which is largest."*

---

## 2. Just-In-Time (JIT) Quest Spawning

**NEVER pre-generate future quests.** Quest N+1 exists ONLY after Quest N has been validated as cleared.

```text
SPAWN → ATTEMPT → EVIDENCE → VALIDATION → LOG → ADAPT → SPAWN NEXT
```

### Quest Spawning Algorithm
1. Read `progress.md`.
2. Identify the single next capability to unlock.
3. Check `learner-profile.md` for stuck patterns, pace, and hint dependency.
4. Select a suitable **Quest Archetype**.
5. Formulate ONE observable goal, clear actions, tight constraints, and observable success criteria.
6. Check for zero spoilers / zero embedded hints.
7. Generate and write exactly ONE quest file.
8. Update state and STOP.

### Quest Archetypes
- **Discovery** — investigate and find out what an unknown command/feature/behavior does.
- **Construction** — build a minimal functional artifact meeting exact specs.
- **Debugging** — identify and fix intentionally broken state/code.
- **Reverse Engineering** — deduce system rules or state transitions from output.
- **Constraint** — solve under strict limits (e.g. single-line command, no loops, specific memory limit).
- **Optimization (Golf)** — refactor to be faster, shorter, or more robust.
- **Boss** — combine multiple unlocked skills in an unscaffolded, high-ambiguity challenge.

### Heuristic Selection Priority
```text
critical gap > missing prerequisite > weak area > difficulty progression > novelty
```

---

## 3. Difficulty Dimensions (Agent Mental Heuristic)

While quest frontmatter uses an integer scalar (`difficulty: 1` to `5`), internal difficulty adjustment considers 5 dimensions:

- **Complexity:** Number of interconnected variables or components.
- **Ambiguity:** Degree of explicit direction provided vs. self-directed discovery.
- **Constraint Count:** Number of operational restrictions.
- **Novelty:** Unfamiliarity of domain context.
- **Evidence Depth:** Granularity and rigor of proof required.

---

## 4. Anti-Spoiler Hint Ladder

**Quest files contain ZERO hints.** Hints are dynamically generated on explicit request only (`/quest hint` or *"give me a hint"*).

```text
No Hint (default)
  ↓ (request 1)
Tier 1 — Direction: Broad conceptual direction; zero solution names or commands.
  ↓ (request 2)
Tier 2 — Search Target: Specific man page, header, tool, or parameter to investigate.
  ↓ (request 3)
Tier 3 — Guided Step: One concrete immediate action (still not full answer).
```

### Adaptation Rules
- **High hint usage (Tier 2–3):** Reduce difficulty of next quest, reinforce prerequisites.
- **Clean independent clears (Tier 0):** Increase difficulty, introduce archetype variety.
- **Emergency Solution:** If blocked after Tier 3, provide solution directly, then spawn a fresh variant quest (different parameters, identical skill) that MUST be cleared independently to advance.

---

## 5. Dummy Data & Sandbox Fixtures (`/quest dd`)

Triggered by: `/quest dd`, `/quest dd <format>`, `/quest dummy`, *"generate sample data"*, *"give me test fixture"*.

When a quest requires testing, querying, or manipulating data and the learner does not have test data, the agent generates a **minimal, zero-spoiler sandbox dataset**.

### Universal Dummy Data Formats
- **Databases & SQL:** Minimal `CREATE TABLE` and `INSERT INTO ...` rows (3–5 realistic rows).
- **Data & APIs:** Clean JSON objects, arrays, or CSV snippets.
- **Text & Regex:** Sample string corpora with matching and non-matching edge-case lines.
- **System & DevOps:** Sample auth/access log lines, mock `.env` or config files.
- **Algorithms:** Sample input arrays/matrices with known edge cases (empty, sorted, inverted).

### Rules for Dummy Data:
1. **Zero Spoilers:** The dummy data provides the *testing environment / input fixtures*, NOT the query, code, or command that solves the quest.
2. **Minimalist:** Exactly enough rows/items to demonstrate success criteria (no 100-line walls of text).
3. **Self-Contained:** Can be directly copy-pasted into SQLite, Postgres, a file, or terminal.
4. Output with a single concise code block and STOP.
