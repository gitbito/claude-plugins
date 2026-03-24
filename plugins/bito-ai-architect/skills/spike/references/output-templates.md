# Output Templates

Read in Phase 4. Apply after all checkpoints are passed.

---

## Universal Rules

1. **Answer first.** TL;DR with the direct answer and confidence level before any detailed findings.
2. **Confidence:** ✅ confirmed from read code | ⚠️ inferred from structure | ❓ speculative.
3. **Every finding cites evidence.** No claim without a source — code reference, architectural observation, or explicit "speculative" label.
4. **Surface what's still unknown.** Unanswered questions and what was tried.
5. **One name per entity.** BitoAI's name for repos and services. Consistently.

---

## Persona Calibration

**Senior Engineer / Architect:** Full evidence log with file:line references, code-level findings, hypothesis evaluations with specific proof. Constraints cited from schema definitions and config.

**Engineering Manager:** Hypothesis verdicts, risk summary, recommended next steps. Evidence summarized as "confirmed from [service]'s code" rather than file:line.

**Product Manager:** Plain English answer to the spike question. Verdicts as "yes we can / no we can't / partially — here's why." Risks as business impact. No file paths.

**Tech Lead:** Hypothesis table, constraints, recommended path with effort signals, follow-up spikes if needed.

---

## Spike Frame Template

```
**Question**: [The specific thing we don't know — phrased as a question]
**Why it matters**: [What decision is blocked by this unknown]
**Hypotheses**: [2-3 plausible answers the team is considering]
**Success criteria**: [What does "answered" look like? What evidence would resolve this?]
**Scope boundary**: [What is explicitly NOT being investigated]
```

---

## Spike Report Template

```markdown
# Spike Report: [Spike Title]

## 1. Spike Frame
- **Question**: [The question this spike set out to answer]
- **Why It Matters**: [What decision was blocked]
- **Time Invested**: [How many queries / how deep the investigation went]

## 2. TL;DR — Answer

[2-3 sentences: the direct answer to the spike question, with confidence level]

## 3. Hypotheses Evaluated

| Hypothesis | Verdict | Confidence | Key Evidence |
|---|---|---|---|
| [Hypothesis 1] | Viable / Not Viable / Partially Viable | ✅ / ⚠️ / ❓ | [One-line evidence summary] |
| [Hypothesis 2] | Viable / Not Viable / Partially Viable | ✅ / ⚠️ / ❓ | [One-line evidence summary] |
| ... | ... | ... | ... |

## 4. Detailed Findings

### Finding 1: [Title]
**Relevance**: [Which hypothesis/question this addresses]
**Evidence**:
- [Specific code reference, file:line, what was found] ✅
- [Architectural observation] ⚠️
**Implication**: [What this means for the decision]

### Finding 2: [Title]
...

## 5. Constraints Discovered
- [Hard constraint 1]: [What it is, where it lives in code, why it matters]
- [Hard constraint 2]: ...

## 6. Risks Identified
| Risk | Severity | Likelihood | Relates To | Evidence |
|---|---|---|---|---|
| [Risk] | H/M/L | H/M/L | [Hypothesis/finding] | ✅/⚠️ |

## 7. Recommendations

### Recommended Path
[What the team should do next, based on findings]

### If [Hypothesis X] Is Chosen:
- **Next steps**: [Concrete actions]
- **Key risks to mitigate**: [From Section 6]
- **Estimated complexity**: [Grounded in what was found]

### If [Hypothesis Y] Is Chosen:
- **Next steps**: [Concrete actions]
- **Key risks to mitigate**: [From Section 6]
- **Estimated complexity**: [Grounded in what was found]

## 8. What We Still Don't Know
- [Remaining unknown 1]: [Why it couldn't be resolved and what would resolve it]
- [Remaining unknown 2]: ...

## 9. Sources & Evidence Log
| # | Source | Type | Query Used |
|---|---|---|---|
| 1 | [repo/file:line] | Code ✅ | [AI Architect query] |
| 2 | [architectural observation] | Inferred ⚠️ | [AI Architect query] |
| ... | ... | ... | ... |
```
