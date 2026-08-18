# Core Vault Templates & Schemas

All YAML frontmatters use plain scalars or quoted strings (no unquoted wikilinks).

---

## 1. `README.md` — Durable Topic Memory & Index

```markdown
---
id: topic-readme
topic: git-basics
type: topic-index
status: active
level: beginner
quests: 12
completed: 8
notes: 5
last_quest: 008-git-branch-switch
---

# Git Basics

## Objective
<one paragraph: what the learner will be able to DO>

## Learning Path
1. [[001-git-init]]
2. [[002-git-status]]
- Future milestone: <plain text, no Wikilink until file exists>

## Unlocked Knowledge
- <bullet of durable fact the learner has demonstrated>

## Completed Quest Archive
- [[001-git-init]] — repository creation
  - [[001a-git-init-golf]] — completed via one-liner   (only if completed)
- [[002-git-status]] — state inspection
```

---

## 2. `learner-profile.md` — Durable Learner State

```markdown
---
id: learner-profile
type: learner-state
topic: git-basics
level: beginner
independence: medium
pace: adaptive
hint_dependency: low
last_updated: 2026-08-18
---

# Learner Profile

## Current Level
<short>

## Strengths
- <bullet>

## Weaknesses
- <bullet>

## Stuck Patterns
- <bullet of recurring failure type>

## Agent Adaptation Rules
- Reduce difficulty after 2+ consecutive fails
- Increase difficulty after 2+ consecutive clean clears
- Reduce hints after repeated independent clears
```

---

## 3. `progress.md` — Hot Runtime State (CRITICAL: < 60 Lines)

```markdown
---
id: progress
type: hot-state
topic: git-basics
status: active
current_quest: 009-git-reset
completed: 8
last_completed: 008-git-branch-switch
level: beginner
independence: medium
---

# Progress

## Current Quest
[[009-git-reset]]

## State
- status: active
- attempt: 1
- hint_tier: 0

## Last 5 Cleared
- [[004-git-commit]]
- [[005-git-log]]
- [[006-git-diff]]
- [[007-git-branch]]
- [[008-git-branch-switch]]

## Active Issues
- None.

## Next Runtime Action
Validate evidence for [[009-git-reset]].
```

**Pruning Rule:** Exactly 5 latest completions stay. On 6th, remove oldest from `progress.md` (it remains safely in `README.md` archive).

---

## 4. Quest File Template (`quests/NNN-<slug>.md`)

```markdown
---
id: 009-git-reset
type: quest
topic: git-basics
number: 9
status: active
difficulty: 2
concepts: [reset, working-tree, staging]
prerequisites: [008-git-branch-switch]
hint_tier: 0
attempt: 1
spawned: 2026-08-18
next: null
---

# Quest 009 — Git Reset

## Goal
<one-sentence observable outcome: what to achieve>

## Do
<the operation the learner must perform; concrete and observable>

## Constraints
- disposable environment only
- no searching for the complete solution
- preserve reproducible evidence

## Success Criteria
You clear this quest when you can demonstrate:
1. <observable outcome A>
2. <observable outcome B>
3. <observable outcome C>
```

**Quest Status Enum:**
`queued | active | paused | blocked | submitted | passed | failed | abandoned | superseded`

**Difficulty Scale:** Integer `1` (introductory) to `5` (complex boss/multi-skill integration).

---

## 5. Learning Note Template (`notes/<topic>-<concept>.md`)

Written ONLY when a quest clears a genuinely durable concept needed later.

```markdown
---
id: git-working-tree
type: learning-note
topic: git-basics
concept: working-tree
filename: git-working-tree.md
source_quests: [001-git-init, 002-git-status]
last_verified: 2026-08-18
---

# Git Working Tree

## Core Idea
<one paragraph: the durable fact>

## How It Was Cleared
<which quest, what observable outcome>
```

**Note Appending (UPSERT):** Append dated section for new discoveries; never create `git-working-tree-2.md`.
```markdown
## Update — 2026-08-18
Quest: [[008-git-branch-switch]]
<new fact>
```
