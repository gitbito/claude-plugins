# Output Templates

Read in Phase 5. Apply after all checkpoints are passed. Use the template matching the ceremony level from Phase 1b.

---

## Universal Rules

1. **Architecture first, tickets second.** The plan must open with the selected approach before any task tables.
2. **Confidence:** ✅ confirmed from read code | ⚠️ inferred from structure | ❓ not confirmed.
3. **Both estimates always present.** Every sized item carries Traditional and Agentic estimates.
4. **Every estimate has a basis.** No sizing without citing the codebase evidence or external research that justifies it.
5. **Cross-cutting concerns are mandatory.** Address every relevant concern — note which were skipped and why.
6. **Surface what was ruled out.** Rejected approaches and excluded scope, with reasons.
7. **One name per entity.** BitoAI's name for repos and services. Consistently.
8. **Approach count follows reality.** The number of approaches in the report reflects how many genuinely distinct options exist — not a quota.

---

## Persona Calibration

**Senior Engineer / Tech Lead:** Full workstream deep dives with repo paths, file references, schema details, API specs. Tickets with acceptance criteria and dependency annotations. Both estimate columns with compression rationale. Cross-cutting concerns in full detail.

**Engineering Manager:** Workstream-level view with team assignments, critical path, dual timeline estimates, coordination risks, and go/no-go milestones. Tickets summarized as counts per workstream.

**Product Manager:** Plain English approach summary. Effort as timeline range ("6-8 weeks traditional, 4-5 weeks with AI tooling"). Risks as user/business impact. Milestones as deliverable capabilities. No file paths or function names.

**VP / Director:** Executive summary: selected approach, total effort (both estimates), timeline, team count, top 3 risks. Architecture diagram. Critical path.

---

## Large Report Template

```markdown
# Implementation Plan: [Epic / Initiative Name]

## 1. Executive Summary

**Ceremony Level**: Large
**Selected Approach**: [Approach name]
**One-line summary**: [What and why]

**Effort (Traditional)**: [Total size / timeline]
**Effort (Agentic)**: [Total size / timeline]
**Teams Involved**: [List]
**Top Risk**: [Single biggest risk in one sentence]

## 2. Context Summary

### System Map
[Condensed from Checkpoint 1]

### Architecture Diagram (Current State)
```
[ASCII diagram]
```

### Closest Existing Analogue
[Which existing flow this builds on]

### External Research Insights
- [Pattern 1]: [Source, relevance]
- [Pitfall 1]: [Source, what to avoid]

## 3. Approach Selection

### Selected: [Approach Name]
[1 paragraph: what it does and why it was chosen]

**Grounded in**: [Existing codebase patterns and external research]

### Architecture Diagram (Target State)
```
[ASCII diagram — NEW vs. MODIFIED, sync vs. async, data flow]
```

### Rejected Alternatives
**[Alternative A]**: [Why rejected]
**[Alternative B]**: [Why rejected]

## 4. Workstream Deep Dives

### Workstream 1: [Name]

**Affected Repos**: [From AI Architect]
**Team Owner**: [Team]

**Database Changes**:
| Table | Change | Details | Migration Strategy |
|---|---|---|---|
| [table] | [type] | [specifics] | [strategy] |

**API Changes**:
| Method | Path | Change Type | Details |
|---|---|---|---|
| [method] | [path] | [type] | [details] |

**New Code**: [modules/classes to create]
**Modified Code**: [existing files and what changes]
**Config / Feature Flags**: [flags needed]
**Testing**: [unit, integration, load requirements]

### Workstream 2: [Name]
...

## 5. Effort Estimates

| Workstream | Traditional | Agentic | Basis | What Drives the Difference |
|---|---|---|---|---|
| [WS 1] | [X-Y eng-days] | [X-Y eng-days] | [Evidence] | [Rationale] |
| **Total** | **[X-Y eng-days]** | **[X-Y eng-days]** | | |

### Agentic Tooling Guidance
**High compression:** [Which tasks and why]
**Low compression:** [Which tasks and why]

## 6. Task Breakdown

### EPIC: [Title]

#### Workstream 1: [Name]

##### Story 1.1: [Milestone]

| Task | Repo | Labels | Trad | Agentic | Blocked By | AC |
|---|---|---|---|---|---|---|
| [title] | [repo] | [labels] | Xd | Xd | [dep] | - [AC] |

## 7. Sequencing & Critical Path

### Visual Timeline
```
[Week-by-week ASCII timeline]
```

**Critical path (Traditional)**: [sequence] = [X weeks]
**Critical path (Agentic)**: [sequence] = [X weeks]

### Milestones & Go/No-Go Points
| Milestone | Target | Criteria | Depends On |
|---|---|---|---|
| [milestone] | Week X | [criteria] | [deps] |

## 8. Cross-Cutting Concerns

### Data Migration
[Details or "Not applicable — no schema changes"]

### Backward Compatibility
[Details]

### Rollback Strategy
| Phase | Can Rollback? | How | Data Implications |
|---|---|---|---|
| [phase] | [yes/partial/no] | [mechanism] | [risk] |

### Observability
[Metrics, alerts, dashboards, log events]

### Performance
[Load, latency targets, testing plan, bottlenecks]

### Security & Access Control
[Permissions, data sensitivity, auth changes]

### Testing Strategy
[Unit, integration, e2e, load, contract]

### Documentation
[Runbooks, API docs, ADRs]

## 9. Risks & Mitigations
| Risk | Likelihood | Impact | Source | Mitigation | Early Warning |
|---|---|---|---|---|---|
| [risk] | H/M/L | H/M/L | [source] | [mitigation] | [signal] |

## 10. Open Questions
| # | Question | Why It Matters | Who Answers | Default |
|---|---|---|---|---|
| 1 | [question] | [impact] | [role] | [default] |

## 11. Scope Boundary
### In Scope (MVP)
- [capability]
### Out of Scope (Follow-on)
- [item]: [why deferred]

## 12. Evidence Log
| # | Source | Type | Finding |
|---|---|---|---|
| 1 | [source] | [type] | [finding] |

## 13. Recommended Follow-ups
| If You Need | Use Skill | For What |
|---|---|---|
| Coding instructions | `bito-feature-plan` | Engineer-level execution |
| Technical design doc | `bito-trd` | Architecture review |
```

---

## Medium Report Template

```markdown
# Implementation Plan: [Epic / Story Name]

## 1. Executive Summary

**Ceremony Level**: Medium
**Selected Approach**: [Approach name]
**One-line summary**: [What and why]

**Effort (Traditional)**: [Total]
**Effort (Agentic)**: [Total]
**Top Risk**: [One sentence]

## 2. Context Summary
[Repos, dependencies, existing analogue, external insights]

## 3. Approach Selection

### Selected: [Approach Name]
[1 paragraph]

### Architecture Sketch
```
[Brief ASCII diagram or text description]
```

### Alternatives Considered
- **[Alt A]**: [Why not chosen — 1-2 sentences]
- **[Alt B]**: [Why not chosen — 1-2 sentences]

## 4. Workstream Detail

### [Workstream Name]
**Repos**: [list]
**Database Changes**: [specifics or "none"]
**API Changes**: [specifics or "none"]
**Key Code Changes**: [files and what changes]
**Feature Flags**: [if needed]
**Testing**: [requirements]

## 5. Task Breakdown

### STORY: [Title]

| # | Task | Repo | Trad | Agentic | Blocked By | AC |
|---|---|---|---|---|---|---|
| 1 | [title] | [repo] | Xd | Xd | none | - [AC] |
| 2 | [title] | [repo] | Xd | Xd | Task 1 | - [AC] |

**Total (Traditional)**: [X days]
**Total (Agentic)**: [X days]

## 6. Sequencing
[Ordered task list with dependency notes and both duration estimates]

## 7. Cross-Cutting Concerns
[Address only what's relevant. Note what was skipped and why.]

## 8. Risks
| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| [risk] | H/M/L | H/M/L | [mitigation] |

## 9. Open Questions
- [Question]: [Who answers, suggested default]

## 10. Scope Boundary
**In Scope**: [list]
**Out of Scope**: [list with reasons]
```

---

## Small Report Template

```markdown
# Implementation Plan: [Story / Ticket Name]

## 1. Summary

**Ceremony Level**: Small
**Repo**: [repo name]
**Approach**: [Name — one sentence describing the path]

**Effort (Traditional)**: [X days]
**Effort (Agentic)**: [X days]

## 2. Context
[Key files, existing pattern to follow, relevant external insight if any]

## 3. Approach

**Selected**: [Name + one sentence]

**Alternatives considered**:
- [Alt A]: [One sentence — why not]
- [Alt B]: [One sentence — why not]
*(Or: "No genuinely distinct alternatives — [brief reason]")*

## 4. Tasks

| # | Task | Files | Trad | Agentic | Blocked By | AC |
|---|---|---|---|---|---|---|
| 1 | [title] | [files] | 1d | 0.5d | none | - [AC] |
| 2 | [title] | [files] | 2d | 1d | Task 1 | - [AC] |

**Total (Traditional)**: [X days]
**Total (Agentic)**: [X days]

## 5. Cross-Cutting (if applicable)
[Only what's relevant — e.g., "Feature flag: [name] needed for safe rollout." Skip sections that don't apply.]

## 6. Risks
- [Risk if any, or "No significant risks identified"]

## 7. Open Questions
- [Question if any, or "None"]
```
