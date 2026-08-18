# Vault Architecture & File Standards

## 1. Directory Structure & Path Anchor

Workspace root (`cwd`) is the canonical filesystem root. All paths resolve relative to it:
`<workspace-root>/learning/<topic>/`

```text
learning/
└── <topic>/
    ├── README.md           # durable topic-level memory & completed quest index
    ├── progress.md         # hot runtime state (<60 lines)
    ├── learner-profile.md  # durable learner state & adaptation rules
    ├── quests/             # spawned quests directory
    │   ├── 001-<slug>.md
    │   ├── 002-<slug>.md
    │   └── ...
    └── notes/              # unlocked durable concept notes
        └── <topic>-<concept>.md
```

### Path Constraints
- **FORBIDDEN:** Nested paths like `learning/learning/<topic>/`, paths outside workspace root, or guessing a different root.
- **Directory Auto-Creation (`mkdir -p`):** Before writing any quest or note in Mode A, verify that target subdirectories (`learning/<topic>/quests/`, `learning/<topic>/notes/`) exist, or create them silently first.

---

## 2. Naming Conventions

| Type | Format | Example |
|---|---|---|
| Topic dir | lowercase kebab-case | `linux-basics`, `python-programming`, `music-theory` |
| Standard quest | `NNN-<slug>.md` (3-digit, kebab-case) | `001-git-init.md`, `009-git-reset.md` |
| Optional variant branch | `NNN<alpha>-<slug>.md` | `009a-git-reset-golf.md`, `009b-git-reset-refactor.md` |
| Remedial branch (blocking) | `NNNr-<slug>.md` or `NNNr1`, `NNNr2`... | `009r-git-reset-remedial.md` |
| Learning note | `<topic-slug>-<concept-slug>.md` | `notes/git-working-tree.md` |

- Forbidden: Spaces, underscores, PascalCase, bare concept names without topic prefix.

---

## 3. Wikilink & Obsidian YAML Compatibility (CRITICAL)

### Obsidian YAML Frontmatter Rule
- **NEVER** place unquoted Wikilinks (`[[...]]`) in YAML frontmatter. Obsidian's YAML parser considers `[` as a YAML flow sequence indicator and will corrupt/error if brackets are bare.
- In frontmatter, use clean plain scalars or quoted strings:
  ```yaml
  # CORRECT:
  current_quest: 009-git-reset
  parent: 009-git-reset
  prerequisites: [008-git-branch-switch]
  next: null
  
  # FORBIDDEN (breaks Obsidian YAML parser):
  current_quest: [[009-git-reset]]
  ```
- Wikilinks (`[[001-git-init]]`, `[[git-working-tree]]`) are strictly for the **Markdown body**.

### No Ghost Nodes
- **NEVER** create a Wikilink to a file that does not yet exist on disk.
- `next:` field in quest frontmatter MUST remain `null` until the child quest file actually exists and is verified on disk.
- Cross-topic links use flat topic-prefixed note IDs: `[[linux-process-model]]` (not path-qualified).

---

## 4. Branch Convergence

- **Optional variants (`a`, `b`, `c`...)**: Non-blocking. Completing `009a` does NOT require doing `009b`. Can converge directly to `010`.
- **Remedial branches (`r`, `r1`, `r2`...)**: Blocking. After `009r` passes, learner MUST re-attempt parent `009` and have it validated before advancing to `010`. NEVER jump `009r → 010`.
- **Branch Frontmatter:**
  ```yaml
  parent: 009-git-reset
  branch: a              # or r
  branch_type: variant   # or remedial
  convergence: mainline  # or parent-reattempt
  ```
- **Archive Rule:** Completed variants are archived in `README.md` as indented sub-items under the parent. Skipped/uncompleted variants MUST NOT be archived.

---

## 5. Dual-Layer Protocol & State Hierarchy

### Dual-Layer Format
Every persistent Markdown file consists of:
1. **Dense YAML frontmatter** — compact, deterministic, machine-readable. No prose.
2. **Human-readable Markdown body**.

### State Authority Hierarchy
```text
progress.md > current quest frontmatter > learner-profile.md > README.md > older history
```
- `progress.md` is the absolute runtime authority. Historical files must not override hot state.
- **Hot:** `progress.md` (always inspect first).
- **Warm:** `learner-profile.md`, current quest, relevant notes.
- **Cold:** `README.md`, completed quests archive (read only on explicit summaries/archive updates).
