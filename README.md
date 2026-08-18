# Quest-Based Learning Agent

An autonomous, game-style learning agent that turns any subject into an OverTheWire / CTF-style quest system backed by a stateful Obsidian Markdown vault.

> **Core Philosophy:** Give the objective. Watch the learner clear it with observable evidence. Spawn the next challenge just-in-time.

---

## 🎮 What This Is

The **Quest-Based Learning Agent** is an agent specification for practical, exploration-driven learning across domains such as:
- **Programming & DevOps:** Python, Rust, Go, Git, Linux, Docker, Kubernetes
- **Cybersecurity:** Web Security, CTF Challenges, Network Analysis
- **Theoretical & Technical:** Mathematics, Music Theory, Languages, Electronics, Data Science

The agent acts as a **Quest Master** and persistent state manager:
1. **Calibration:** Starts with a single practical task to gauge starting level (no long placement tests).
2. **Just-In-Time Spawning:** Quests are generated one-by-one only after the prior quest is validated.
3. **Observable Evidence:** Progression requires actual output, command logs, or code execution (zero theoretical essay questions).
4. **Anti-Spoiler Hints:** Quests contain zero hints; dynamic hints are unlocked one tier at a time on explicit request.
5. **Stateful Vault:** Tracks progress, active issues, and unlocked concepts directly in your Obsidian vault.

---

## ⚡ Quick Command Triggers

| Command | Action |
|---|---|
| `/quest start <topic>` or `Learn <topic>` | Calibrate and spawn Quest 001 for a new topic |
| `/quest next`, `continue`, `next quest` | Batch state transition and spawn Quest N+1 |
| `/quest hint` | Request the next hint tier (Tier 1 $\rightarrow$ 2 $\rightarrow$ 3) |
| `/quest dd` or `/quest dummy` | Generate minimal dummy data / fixtures (SQL, CSV, JSON, logs, regex test text) |
| `/quest evidence <output>` | Submit terminal output, code, or results for validation |
| `/quest status` | View current active quest and progress |
| `/quest pause` | Save state snapshot with revealed hint memory |
| `/quest resume <topic>` or `BOOTSTRAP` | Cold-start resume using the 4-line output mask |
| `/quest switch <topic>` | Freeze active topic and switch context to a new topic |
| `/quest golf` or `/quest replay` | Submit an optimized or alternative solution for a cleared quest |

---

## 📁 Vault Architecture

Each topic maintains an isolated, clean directory structure in your Obsidian vault:

```text
<workspace-root>/
├── SKILL.md            # complete self-contained skill specification
├── README.md           # repository overview
└── learning/
    └── <topic>/
        ├── README.md           # durable topic index & completed quest archive
        ├── progress.md         # hot runtime state (<60 lines, last 5 clears)
        ├── learner-profile.md  # adaptation rules & recurring patterns
        ├── quests/             # spoiler-free quest files
        │   ├── 001-<slug>.md
        │   ├── 002-<slug>.md
        │   └── ...
        └── notes/              # unlocked concept summaries (upserted)
            └── <topic>-<concept>.md
```

### State Hierarchy
- **Hot:** `progress.md` (runtime source of truth, strictly kept under 60 lines).
- **Warm:** `learner-profile.md`, current active quest, relevant concept notes.
- **Cold:** `README.md` and historical quests archive.

---

## 🔄 Two-Phase Turn Control

The agent strictly enforces a two-phase conversational turn structure:

```text
Turn A — Validation
   Learner submits evidence
   Agent validates: PASS / FAIL / GAP
   STOP (Does not spawn N+1 yet)

Turn B — Continuation
   Learner says "continue" / "/quest next"
   Agent executes atomic write order:
     1. Write & verify Quest N+1
     2. Update progress.md & README archive
     3. Patch parent Quest N's `next:` link
   Agent presents Quest N+1
   STOP
```

---

## 📚 Specification & Compatibility

The full specification lives in [`SKILL.md`](file:///home/stoy/.agents/skills/Quest-Based-Learning/SKILL.md). It is self-contained and fully compatible with **skills.sh**, Antigravity, and standard Agent Skills registries.
