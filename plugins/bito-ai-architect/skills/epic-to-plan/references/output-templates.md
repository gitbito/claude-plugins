# Output Templates

Read in Phase 5. Apply after all checkpoints are passed.

---

## Universal Rules

1. **Architecture first, tickets second.** The plan must open with the selected approach and architecture before any task tables.
2. **Confidence:** ✅ confirmed from read code | ⚠️ inferred from structure | ❓ not confirmed.
3. **Both estimates always present.** Every sized item carries Traditional and Agentic estimates.
4. **Every estimate has a basis.** No sizing without citing the codebase evidence or external research that justifies it.
5. **Cross-cutting concerns are mandatory.** Every plan addresses rollback, observability, migration, testing, and security — never deferred to "figure it out during implementation."
6. **Surface what was ruled out.** Rejected approaches and excluded scope, with reasons.
7. **One name per entity.** BitoAI's name for repos and services. Consistently.

---

## Persona Calibration

**Senior Engineer / Tech Lead:** Full workstream deep dives with repo paths, file references, schema details, API specs. Tickets with acceptance criteria and dependency annotations. Both estimate columns with compression rationale. Cross-cutting concerns in full detail.

**Engineering Manager:** Workstream-level view with team assignments, critical path, dual timeline estimates, coordination risks, and go/no-go milestones. Tickets summarized as counts per workstream with blocked/blocking annotations.

**Product Manager:** Plain English approach summary. Effort as timeline range ("6-8 weeks traditional, 4-5 weeks with AI tooling"). Risks as user/business impact. Milestones as deliverable capabilities, not technical tasks. No file paths or function names.

**VP / Director:** Executive summary: selected approach, total effort (both estimates), timeline, team count, top 3 risks. Architecture diagram. Critical path. Go/no-go decision points.

---

## Implementation Plan Template

```markdown
# Implementation Plan: [Epic / Feature Name]

## 1. Executive Summary

**Selected Approach**: [Approach name]
**One-line summary**: [What this approach does and why it was chosen]

**Effort (Traditional)**: [Total size / timeline]
**Effort (Agentic)**: [Total size / timeline]
**Teams Involved**: [List]
**Top Risk**: [Single biggest risk in one sentence]

## 2. Context Summary

### System Map
[Condensed from Checkpoint 1 — affected services, topology, ownership]

### Architecture Diagram (Current State)
```
[ASCII diagram showing current system relevant to this epic]
```

### Closest Existing Analogue
[Which existing flow this builds on, and what it teaches]

### External Research Insights
- [Pattern 1]: [Source, relevance] 
- [Pitfall 1]: [Source, what to avoid]
- [Standard/protocol]: [If applicable]

## 3. Approach Selection

### Selected: [Approach Name]
[1 paragraph: what it does and why it was chosen over alternatives]

**Grounded in**: [Which existing codebase patterns and external research this follows]

### Architecture Diagram (Target State)
```
[ASCII diagram showing the system after this plan is implemented]
[Label: NEW vs. MODIFIED components]
[Label: sync vs. async boundaries]
[Label: data flow direction and protocol]
```

### Rejected Alternatives

**[Alternative A Name]**: [1-2 sentences: what it was and why it was rejected]
**[Alternative B Name]**: [1-2 sentences: what it was and why it was rejected]

## 4. Workstream Deep Dives

### Workstream 1: [Name]

**Affected Repos**: [Exact repo names from AI Architect]
**Team Owner**: [Team]

**Database Changes**:
| Table | Change | Details | Migration Strategy |
|---|---|---|---|
| [table] | Add column / New table / Index | [Specifics] | [Online DDL / backfill / etc.] |

**API Changes**:
| Method | Path | Change Type | Details |
|---|---|---|---|
| [GET/POST/...] | [/path] | New / Modified | [Request/response shape changes] |

**New Code**:
- [Module/class/package]: [Purpose] — in [repo]

**Modified Code**:
- [Existing file/module]: [What changes and why] — in [repo]

**Config / Feature Flags**:
- [Flag name]: [What it gates, default value, rollout plan]

**Testing**:
- [Unit]: [Key areas]
- [Integration]: [Cross-service scenarios]
- [Load]: [If applicable — targets and plan]

### Workstream 2: [Name]
...

## 5. Effort Estimates

### By Workstream

| Workstream | Traditional | Agentic | Basis | What Drives the Difference |
|---|---|---|---|---|
| [WS 1] | [X-Y eng-days] | [X-Y eng-days] | [Evidence] | [Compression rationale] |
| [WS 2] | [X-Y eng-days] | [X-Y eng-days] | [Evidence] | [Compression rationale] |
| ... | ... | ... | ... | ... |
| **Total** | **[X-Y eng-days]** | **[X-Y eng-days]** | | |

### Agentic Tooling Guidance

**High compression (plan from Agentic column):**
- [Task type 1]: [Why — e.g., CRUD boilerplate, follows existing pattern]
- [Task type 2]: [Why — e.g., test scaffolding, migration scripts]

**Low compression (plan from Traditional column):**
- [Task type 1]: [Why — e.g., novel state machine, security logic]
- [Task type 2]: [Why — e.g., performance optimization, concurrency]

## 6. Task Breakdown

### EPIC: [Epic Title]

#### Workstream 1: [Name]

##### Story 1.1: [User-facing capability or milestone]

| Task | Repo | Labels | Trad | Agentic | Blocked By | AC |
|---|---|---|---|---|---|---|
| [Task title] | [repo] | backend | 2d | 1d | none | - [AC 1]<br>- [AC 2] |
| [Task title] | [repo] | data | 1d | 0.5d | Task above | - [AC 1] |

##### Story 1.2: [Next milestone]
...

#### Workstream 2: [Name]
...

## 7. Sequencing & Critical Path

### Visual Timeline

```
Week 1-2          Week 3-4          Week 5-6          Week 7-8
─────────────────────────────────────────────────────────────────
[Phase 1 tasks]──▶[Phase 2 tasks]──▶[Integration]───▶[QA & Launch]
                  [Parallel work]──▶[Converges]─────┘
[Feature flags]   [UI scaffolding]─▶[UI complete]──▶[E2E tests]
```

**Critical path (Traditional)**: [Task A] → [Task B] → ... → [Final task] = [X weeks]
**Critical path (Agentic)**: [Task A] → [Task B] → ... → [Final task] = [X weeks]

### Milestones & Go/No-Go Points

| Milestone | Target | Go/No-Go Criteria | Depends On |
|---|---|---|---|
| [Milestone 1] | Week X | [What must be true to proceed] | [Prerequisites] |
| [Milestone 2] | Week X | [What must be true to proceed] | [Milestone 1] |

## 8. Cross-Cutting Concerns

### Data Migration
- **What moves**: [Entities, volumes, transformation needed]
- **Strategy**: [Online migration / dual-write / backfill / etc.]
- **Zero-downtime plan**: [How to avoid service disruption]
- **Rollback**: [How to revert the migration if needed]

### Backward Compatibility
- **Breaking changes**: [List any, or "none"]
- **Versioning strategy**: [API versioning, contract negotiation]
- **Incremental deploy order**: [Which pieces can ship independently]

### Rollback Strategy
| Phase | Can Rollback? | How | Data Implications |
|---|---|---|---|
| [Phase 1] | Yes/Partial/No | [Mechanism] | [Data loss risk?] |
| [Phase 2] | Yes/Partial/No | [Mechanism] | [Data loss risk?] |

### Observability
- **New metrics**: [What to emit, where]
- **New alerts**: [Conditions, thresholds, escalation]
- **New dashboards**: [What to monitor during and after rollout]
- **Log events**: [Key events to log for debugging]

### Performance
- **Expected load**: [Requests/sec, data volume]
- **Latency targets**: [P50, P95, P99]
- **Load testing plan**: [What, when, targets]
- **Known bottlenecks**: [From codebase analysis and external research]

### Security & Access Control
- **New permissions**: [Roles, scopes, gates]
- **Data sensitivity**: [PII handling, encryption needs]
- **Auth changes**: [New middleware, token scopes, etc.]

### Testing Strategy
- **Unit**: [Coverage targets, key areas]
- **Integration**: [Cross-service scenarios to test]
- **E2E**: [User journeys to validate]
- **Load**: [Performance targets and test infrastructure]
- **Contract**: [API contract validation between services]

## 9. Risks & Mitigations

| Risk | Likelihood | Impact | Source | Mitigation | Early Warning |
|---|---|---|---|---|---|
| [Risk from codebase] | H/M/L | H/M/L | ✅ Code | [Mitigation] | [Signal] |
| [Risk from external research] | H/M/L | H/M/L | ⚠️ Industry | [Mitigation] | [Signal] |
| [Coordination risk] | H/M/L | H/M/L | ⚠️ Architecture | [Mitigation] | [Signal] |

## 10. Open Questions

| # | Question | Why It Matters | Who Should Answer | Suggested Default |
|---|---|---|---|---|
| 1 | ... | [What decision it blocks] | [Role/team] | [Default if no answer] |

## 11. Scope Boundary

### In Scope (MVP)
- [Capability 1]
- [Capability 2]

### Out of Scope (Follow-on)
- [Deferred item 1]: [Why deferred, when to revisit]
- [Deferred item 2]: [Why deferred, when to revisit]

## 12. Evidence Log

| # | Source | Type | Finding |
|---|---|---|---|
| 1 | [repo/file:line] | Code ✅ | [What was found] |
| 2 | [dependency graph] | Architecture ⚠️ | [What was found] |
| 3 | [URL / blog / case study] | External Research ⚠️ | [What was found] |
| ... | ... | ... | ... |

## 13. Recommended Follow-ups

| If You Need | Use Skill | For What |
|---|---|---|
| Step-by-step coding instructions for a workstream | `bito-feature-plan` | Detailed engineer-level task execution |
| Formal technical design document | `bito-trd` | Architecture review or RFC |
```
