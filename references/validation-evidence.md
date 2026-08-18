# Evidence Validation & Skill Unlock Model

## 1. Observable Evidence Standard

A quest is CLEARED when the learner submits observable evidence meeting all success criteria.

- **Weak Evidence (REJECT):** *"I ran the command and it worked"*, *"I understand now"*.
- **Strong Evidence (ACCEPT):** Terminal output, command execution logs, test outputs, file diffs, explicit structured solution blocks.

**The Golden Rule:** The agent NEVER asks the learner to write essays, explain concepts, predict hypothetical scenarios, or prove theoretical transfer. Observable evidence alone clears the quest.

---

## 2. Domain-Specific Evidence Requirements

| Domain | Required Evidence Format |
|---|---|
| **Programming** | Code snippet + execution output / passing test assertion |
| **Linux & DevOps** | Exact CLI commands + stdout/stderr + verified state diff |
| **Cybersecurity** | Lab output, exploit/patch artifact + reproducible methodology |
| **Mathematics** | Step-by-step mathematical derivation + final boxed answer `[ANSWER: ...]` |
| **Music Theory** | ABC notation, MIDI dump, chord chart array, or structured voice-leading analysis |
| **Languages** | Sentence production meeting constraints + grammar transformation table |
| **Electronics & IoT** | Circuit schematic / pinout + measurement readouts + logic state table |
| **Data & Science** | Raw input snippet + query/script + exact tabulated output |

---

## 3. Validation Evaluation & Responses

### Evaluation Enum
```yaml
result: pass | fail | gap
```

- **PASS:** Evidence strictly matches all success criteria. Quest cleared.
  ```text
  PASS — Quest 009 cleared.
  Say `continue` or `/quest next` to advance.
  ```
- **FAIL:** Evidence does not meet criteria or is incorrect.
  ```text
  FAIL — Quest 009.
  The output shows staging status X, but criterion requires Y. Try again.
  ```
- **GAP:** Partial evidence provided; exactly one missing piece.
  ```text
  GAP — Quest 009.
  Missing evidence: Provide the output of `git status -s` after running your command.
  ```

*No generic praise ("Great job!", "You're amazing!"). Pure operational clarity.*

---

## 4. Failure Classification & Remediation

```yaml
failure: misunderstanding | missing-prerequisite | execution-error | environment-error | overcomplexity
```

- **1st Failure:** State the precise mismatch in one concise sentence. Learner re-attempts.
- **2nd Consecutive Failure:** Offer a remedial branch (`NNNr-` or `NNNr1-`) breaking down the blocking prerequisite into smaller sub-challenges.
- **Remedial Convergence:** Completing the remedial quest unlocks the right to re-attempt the parent quest. It never skips straight to mainline.

---

## 5. Skill Unlock Model

```yaml
skill: locked | unlocked | mastered
```

- **locked:** Concept not yet encountered.
- **unlocked:** Demonstrated in at least one standard quest.
- **mastered:** Demonstrated cleanly (Hint Tier 0) in a Boss quest integrating multiple concepts.

---

## 6. Replay, Code Golf & Optimization Delta

Learners can replay cleared quests for optimization, alternative methods, or shorter code (`/quest golf` or `/quest replay`).

- **No delta:** Reply `**No delta detected.**`
- **Meaningful delta:** Append delta frontmatter and summary to existing quest file:
  ```yaml
  delta:
    efficiency: improved
    clarity: improved
    robustness: improved
  ```
