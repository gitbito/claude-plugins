# Output Templates

Read in Phase 4. Apply after all checkpoints are passed.

---

## Universal Rules

1. **Recommendation first.** GO / GO WITH CONDITIONS / NO-GO / DEFER — with 3-5 sentence rationale before any tables.
2. **Confidence:** ✅ confirmed from read code | ⚠️ inferred from structure | ❓ speculative.
3. **Every effort estimate has a basis.** No sizing without citing the codebase evidence that justifies it.
4. **Always evaluate "Do Nothing."** The cost of inaction is part of every feasibility analysis.
5. **Surface surprises prominently.** Hidden complexity goes in the Executive Summary, not buried in tables.
6. **One name per entity.** BitoAI's name for repos and services. Consistently.

---

## Persona Calibration

**Staff Engineer / Architect:** Full blast radius with repo-level detail, effort breakdown by area with code-level basis (both Traditional and Agentic estimates), risk register with evidence references, alternatives comparison with technical trade-offs.

**Engineering Manager:** Service-level impact, team-level effort estimates (both Traditional and Agentic with guidance on which applies to their team), coordination risks highlighted, critical path with team dependencies, sizing as sprint estimates where possible.

**Product Manager:** Plain English recommendation. Effort as relative magnitude ("this is a quarter-long initiative, not a sprint" — with a note on how AI tooling might compress it). Risks as business impact ("if X fails, Y users are affected"). No file paths or function names.

**VP / Director:** Executive summary table: recommendation, effort (both estimates), risk, timeline, team count. Alternatives comparison as a decision matrix. Top 3 risks with mitigation.

**CTO:** Strategic framing. Architectural risk implications. Build vs. buy analysis if applicable. Long-term maintenance cost signals. External industry patterns that validate or challenge the approach.

---

## Feasibility Report Template

```markdown
# Feasibility & Impact Analysis: [Proposal Name]

## 1. Executive Summary

**Recommendation**: [GO / GO WITH CONDITIONS / NO-GO / DEFER]

[3-5 sentences: the bottom line. Is this feasible? At what cost? What's the biggest risk? Note any surprises from external research.]

## 2. Proposal Summary
- **What**: [The proposal in one paragraph]
- **Why**: [Business value / motivation]
- **Decision-Maker**: [Who this report is for]
- **Decision Criteria**: [What the stakeholders care about — effort, timeline, risk]

## 3. Impact Map

### Services Affected
| Service / Repo | Change Type | Degree | Team / Owner | Confidence |
|---|---|---|---|---|
| [service] | [New / Modify / Config] | Heavy / Medium / Light | [team] | ✅ / ⚠️ |

### Blast Radius
```
[Proposal]
  ├── [Direct: Service A — heavy modification]
  │     ├── [Indirect: Service B — depends on A's API]
  │     └── [Indirect: Service C — shares A's database]
  ├── [Direct: Service D — light touch]
  └── [Shared: Cache X — used by 5 services]
```

### Shared Resources at Risk
| Resource | Type | Used By (count) | Change Required | Risk |
|---|---|---|---|---|
| [Database X] | PostgreSQL | 3 services | Schema migration | Medium |
| ... | ... | ... | ... | ... |

### Architectural Health Signals
| Affected Area | Health Signal | Source | Impact on This Proposal |
|---|---|---|---|
| [Service / area] | [Tech debt / incident history / scaling constraint] | ✅ / ⚠️ | [How it affects risk or approach] |

## 4. External Research Insights

### Industry Patterns
- [Pattern 1]: [Who used it, what happened, relevance to this proposal] — Source: [link/reference]
- [Pattern 2]: ...

### Known Pitfalls
- [Pitfall 1]: [What went wrong at Company X, at what scale, relevance] — Source: [link/reference]
- [Pitfall 2]: ...

### Standards & Protocols
- [Any relevant industry standards, compliance requirements, or established protocols]

### Synthesis
[2-3 sentences: what external research means for this proposal — patterns to adopt, pitfalls to guard against, build-vs-buy signals]

## 5. Feasibility Assessment

### Technical Viability: [VIABLE / VIABLE WITH CAVEATS / NOT VIABLE]
- [Key finding 1 — with evidence reference] ✅
- [Key finding 2 — with evidence reference] ⚠️
- [Key finding 3 — informed by external research] ⚠️
- **Blockers**: [Hard blockers, if any]
- **Caveats**: [Conditions that must be true for viability]

### Effort Estimate

| Area | Traditional | Agentic (Cursor / Claude Code) | Basis | What Drives the Difference |
|---|---|---|---|---|
| Schema / data model changes | [size] | [size] | [Evidence: current schema, migration complexity] | [e.g., migrations are boilerplate-heavy] |
| Service logic changes | [size] | [size] | [Evidence: handler count, branching complexity] | [e.g., novel state machine vs. CRUD extension] |
| API / contract changes | [size] | [size] | [Evidence: versioning pattern, surface area] | [e.g., existing pattern makes this mechanical] |
| Frontend changes | [size] | [size] | [Evidence: component count, design system] | [e.g., component library covers most needs] |
| Testing | [size] | [size] | [Evidence: current coverage, test patterns] | [e.g., test scaffolding compresses well with AI] |
| Migration / backfill | [size] | [size] | [Evidence: record count, schema change type] | [e.g., script gen compresses, validation is manual] |
| **Total** | **[size]** | **[size]** | | |

**Traditional** = experienced engineer, manual code, own review, hand-written tests.
**Agentic** = experienced engineer directing AI tooling (Cursor, Claude Code, etc.), still reviewing and validating.

**Where agentic tooling compresses effort in this proposal:**
- [Specific area 1 and why]
- [Specific area 2 and why]

**Where agentic tooling helps less:**
- [Specific area 1 and why — e.g., novel logic, security-sensitive, concurrency]

### Risk Register
| Risk | Severity | Likelihood | Area | Evidence | Mitigation |
|---|---|---|---|---|---|
| [Risk 1] | H/M/L | H/M/L | [Technical/Coordination/Operational] | [Code reference or external source] ✅/⚠️ | [Suggested mitigation] |
| [Risk 2 — from external research] | H/M/L | H/M/L | [Area] | [Industry post-mortem / case study] ⚠️ | [Suggested mitigation] |

### Dependencies & Sequencing
| Prerequisite | Owner | Status | Blocks |
|---|---|---|---|
| [Prerequisite 1] | [Team] | [Exists / Needs work / Unknown] | [What it blocks] |
| ... | ... | ... | ... |

**Critical path**: [Shortest sequence from start to done]

## 6. Alternatives Comparison

| Dimension | [Option A: Proposal] | [Option B: Alternative] | [Option C: Do Nothing] |
|---|---|---|---|
| Effort (Traditional) | [S/M/L/XL] | [S/M/L/XL] | — |
| Effort (Agentic) | [S/M/L/XL] | [S/M/L/XL] | — |
| Risk | [H/M/L] | [H/M/L] | [H/M/L] |
| Timeline (Traditional) | [estimate] | [estimate] | — |
| Timeline (Agentic) | [estimate] | [estimate] | — |
| Value delivered | [H/M/L] | [H/M/L] | [describe cost of inaction] |
| External validation | [Pattern match?] | [Pattern match?] | — |
| Fits decision criteria? | [Yes/Partially/No] | [Yes/Partially/No] | [Yes/Partially/No] |

### Alternative Details

**Option B: [Alternative Name]**
[1 paragraph: what it is, why it's an alternative, key trade-off vs. the proposal. Note if informed by external research — e.g., a pattern used successfully at Company X.]

**Option C: Do Nothing**
[1 paragraph: what happens if the team doesn't do this. Is there a cost to inaction?]

## 7. Conditions for GO

If the recommendation is GO or GO WITH CONDITIONS:
- [ ] [Condition 1: e.g., "Shared library team must extend X before work begins"]
- [ ] [Condition 2: e.g., "Schema migration must be tested against production-scale data"]
- [ ] [Condition 3: e.g., "Team B must confirm API contract change is acceptable"]

## 8. Recommended Next Steps

| If Decision Is | Next Action | Skill to Use |
|---|---|---|
| GO | Break into epics and stories | Epic / Story Breakdown |
| GO | Detailed technical design | TRD |
| GO WITH CONDITIONS | Resolve conditions, then proceed | [Depends on condition] |
| DEFER | Revisit in [timeframe] with [new info] | — |
| NO-GO | Consider alternatives or descope | — |

## 9. Open Questions
- [Question 1]: [What's unknown and who can resolve it]
- [Question 2]: ...

## 10. Evidence Log
| # | Source | Type | Finding |
|---|---|---|---|
| 1 | [repo/file:line] | Code ✅ | [What was found] |
| 2 | [dependency graph] | Architecture ⚠️ | [What was found] |
| 3 | [URL / blog post / case study] | External Research ⚠️ | [What was found] |
| ... | ... | ... | ... |
```
