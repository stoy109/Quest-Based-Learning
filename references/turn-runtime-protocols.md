# Turn Control, Runtime Modes & Transaction Protocols

## 1. Two-Phase Conversational Turn Control

Distinguish **conversational turns** (visible to learner) from **tool execution sequences** (internal file ops).

### Turn A — Validation Turn
1. Inspect submitted evidence against criteria.
2. Output PASS / FAIL / GAP.
3. If FAIL/GAP, specify the exact mismatch or missing item.
4. **STOP.** (MUST NOT spawn Quest N+1 in Turn A).

### Turn B — Continuation Turn (After explicit `continue`, `/quest next`, *"advance"*)
1. Batch internal state transitions.
2. Spawn Quest N+1.
3. Verify files on disk.
4. Present Quest N+1 to learner.
5. **STOP.**

---

## 2. Atomic Write Order & Transaction Integrity

When advancing to Quest N+1, execute mutations in this strict order:

```text
1. Prepare Quest N+1 in memory (with next: null).
2. Write learning/<topic>/quests/NNN-<slug>.md.
3. Verify Quest N+1 exists and is readable on disk.
4. ONLY AFTER verification: mutate learning/<topic>/progress.md.
5. Update learning/<topic>/README.md archive and learning notes.
6. Patch parent Quest N's frontmatter: `next: null` -> `next: <new-quest-id>`.
7. Verify all dependent mutations.
```

### Failure & Rollback
- If Step 2 or 3 fails: **ABORT** all subsequent writes. Do not mutate `progress.md` or `README.md`.
- In Mode A: report failure and fall back to Mode B (raw markdown blocks with `<!-- FILE: path -->`).
- Never claim persistence if a write failed.

---

## 3. Session Bootstrap & Output Mask

### Formal Handshake
```text
BOOTSTRAP
topic=<topic>
state=progress.md
mode=resume
```

Natural language triggers like *"Resume Git topic"*, `/quest resume git`, *"continue rust"*, *"resume my session"* are treated as a formal bootstrap.

### 4-Line Bootstrap Output Mask (CRITICAL)
The first message returned after a successful bootstrap MUST follow this exact format:

```text
[TOPIC: <topic>] — Level <level>
Active Quest : [[<current_quest>]] (Attempt <n>, Hint Tier <n>)
Objective    : <1 sentence objective>
State        : Awaiting evidence or action.
```

- If paused: `State : Paused — awaiting resume action.`
- If blocked: `State : Blocked — awaiting resolution.`
- **FORBIDDEN:** Greetings, polite filler (*"Hello!", "Welcome back!"*), or commentary before/after this 4-line block.

---

## 4. Pause / AFK Protocol

Triggered by: `/quest pause`, *"I'll continue tomorrow"*, *"pause this"*, *"save progress"*.

1. Set quest frontmatter: `status: paused`, record `revealed_hints: [...]`.
2. Update `progress.md` snapshot keeping within the 60-line limit.
3. Output: `Resuming Quest <NNN> at Hint Tier <N>. Continue when ready.`
4. **On Resume:** Never repeat already revealed hints. If `revealed_hints: [1, 2]` and learner asks for a hint, immediately serve Tier 3.

---

## 5. Topic Context Switching

Triggered by: `/quest switch <new-topic>`, *"Switch to Linux"*, *"I want to learn Rust first"*.

1. Identify currently active topic.
2. Auto-freeze active topic state (set `status: paused`, `pause_reason: topic-switch` in active quest & `progress.md`).
3. Set active pointer: `active_topic: <new-topic>`, `active_topic_root: learning/<new-topic>/`.
4. Load or initialize target topic's `progress.md`.
5. Isolate all subsequent file reads/writes to `learning/<new-topic>/`.
