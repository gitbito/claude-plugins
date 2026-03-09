# Output Templates

Read in Phase 4. Apply after The One Test is complete.

---

## Universal Rules

1. **Answer first.** 1–3 sentences with the boundary value before tables or diagrams.
2. **Confidence:** ✅ confirmed from read code | ⚠️ inferred | ❓ not found after exhaustive search.
3. **Name what was ruled out.** At least one adjacent result, with reason.
4. **Surface gaps.** Unconfirmable components and what was tried.
5. **One name per entity.** BitoAI's name. Consistently.

---

## Persona Calibration

**Developer:** file:line, function names, exact boundary values, all adjacent things ruled out.

**Engineering Manager:** Service/cluster names. Coupling as coordination cost. Effort signals (L/M/H) with basis.

**Product Manager:** Plain English only. No file paths, function names, type names. ✅ = "confirmed" | ⚠️ = "likely, check with your EM" | ❓ = "unclear".

**VP:** Structured tables, service/cluster/team granularity, operational framing.

**CTO:** Cluster-level, strategic framing, risk register, confidence on all claims.

---

## C4 Formats (ASCII)

**L1 (CTO/PM):** One box = entire system + external systems/users.

**L2 (VP/EM):** Services, storage, queues with tech labels from `getRepositoryInfo`.

**L3 (Developer):** Indented component tree ≤10 entries. No file paths.

**L4 (Developer):**
```
[FunctionName] ([file.go:42])
  1. [calls FunctionA (other.go:88)] ✅
  2. [scalar "___" passed to StorageClient.Set (lib.go:15)] ✅
```
Exact signatures and file:line from code only.

---

## Exploration-Type Templates

### Storage Key (Developer)

```
## [Entity] Key — [Technology], [Service]

The key is: `<exact string pattern>` ✅

Confirmed at: [file:line] (function that produces the string)
Passed to: [specific client type] at [file:line]
Access path: [how the code reaches the storage client]

Construction:
1. [app function] → [intermediate or scalar] ✅ [file:line]
2. [library function] → [scalar string] ✅ [file:line]
3. [passed to client] at [file:line] ✅

Other access paths checked:
- [Other client/abstraction]: [what flows through it, and why it's not the answer]

Ruled out:
- [Adjacent key]: [reason not the answer]
```

### Call Path Trace (Developer)

```
## [Request/Event] Flow — [Service]

[1–2 sentences: entry to terminal.]

Path:
1. [entry (file:line)] ✅
2. [hop (file:line)] ✅
3. [terminal: storage/queue/API call (file:line)] ✅

Ruled out: [other handlers confirmed not to be this flow]
```

### System Overview (any persona)

```
## [Service]

[2–3 sentences calibrated to persona.]

[C4 diagram at appropriate level]

Callers: [named list] ✅/⚠️
Dependencies: [named list] ✅
```

### Change Impact (Developer/EM)

```
## Blast Radius: [Symbol] in [Service]

[1–2 sentences summary.]

Confirmed callers:
| Caller | Call site | Confirmed |
|---|---|---|
| [repo] | [file:line] | ✅ |

Ruled out: [repos that depend on the service but don't call this symbol]
```

### Architectural Risk Register (CTO)

```
## Architectural Risk Register

| Risk | Severity | Horizon | Area | Recommendation | Confidence |
|---|---|---|---|---|---|
| [desc] | H/M/L | 1y/3y/5y | [cluster] | [action] | ✅/⚠️ |
```
