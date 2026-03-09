---
name: codebase-explorer
version: "11.0"
description: >-
  Use when any user — developer, engineering manager, product manager, CTO, VP,
  or anyone curious about the system — wants to understand the codebase.
  Triggers: 'explain this service', 'how does X work', 'what does Y depend on',
  'show me the architecture', 'what APIs does this expose', 'how would I change X',
  'what owns this feature', 'give me a tech overview', 'onboard me to this system',
  'what are the risks in this area', 'call path for X', 'what touches Y',
  'is this safe to delete', 'what does our codebase look like', 'show me a C4 diagram'.
  Works for any depth: high-level executive summary through line-level code traces.
---

# Codebase Explorer

## THE ONE TEST — Read This First

**Before producing any output, you must be able to complete all three lines from code you have read:**

> **Line 1 — The scalar:** "The exact value that crosses the storage/transport boundary is: `___`"
> **Line 2 — The producer:** "I read the function at `[file:line]` that produces this scalar."
> **Line 3 — The client:** "I read the function body of `[client method name]` at `[file:line]` and confirmed it issues a `[protocol/technology]` call — not an in-process operation."

Every blank must be filled from code actually read. Not inferred. Not assumed from the client type's name.

**Line 3 is mandatory and cannot be skipped.** The name of a client type does not prove it uses a specific technology. You must read the client's function body to confirm it makes a network call, DB call, or protocol-level operation to the named technology. A client named "RedisCache" could wrap an in-process cache. A client named "BigCache" could call Redis internally. The only way to know is to read it.

**A struct is not a scalar.** If Line 1 contains a struct, composite key, or object with fields, it is wrong. The scalar is what the confirmed network/protocol call receives — follow the transformation until you reach it.

**A wrapper boundary is not the storage boundary.** The storage boundary is where a scalar crosses into a network socket, DB driver, or protocol buffer — not where it enters a wrapper function.

**No result at one client does not mean absent from the technology.** The same technology may be accessed through a direct client, a wrapper, a DAL, or a cached-session abstraction. Check all of them before concluding absence.

---

## Workflow

```
Phase 1: State what you are looking for in The One Test format
    ↓
Phase 2: Search — topology, then code
    │   After every result: can I fill in The One Test? If no, keep searching.
    │   After every negative result: check other abstractions before concluding absent.
    ↓
Phase 3 (optional): Knowledge context for WHY questions
    ↓
Phase 4: Output — only after The One Test is completable from read code
```

---

## Phase 1: Decompose

Write before any tool calls:

- **Entity:** The specific thing being asked about.
- **Boundary value:** What does the answer look like at the storage/transport boundary — a string? A number? Not a struct.
- **Scope:** Service + specific technology layer. Note: the same technology may have multiple access paths in this codebase.
- **Adjacent traps:** Wrong-but-plausible results to watch for: values at the wrong layer, values for adjacent entities, structs that look like answers but are intermediates.
- **One Test blank:** "The exact value that crosses the boundary is: `___` from `[file:line]` passed to `[client type]`."

---

## Phase 2: Search

### Step 2.0 — For layer-specific questions: map all access paths first

Before searching for values, identify every way this codebase reaches the target technology:
1. `getRepositoryInfo(repo, detailLevel: "summary")` — all storage/cache dependencies
2. `searchCode(pattern: "<technology keyword>", fileNamesOnly: true, repositories: [repo])` — all related files; read imports to find every client type and wrapper
3. Look for: direct clients, wrappers, adapters, DAL patterns, cached-session patterns

Record all access paths. You will check the target entity through each one.

**Per-path rule:** When the target entity is found via multiple access paths, each path requires its own complete One Test (all three lines) before you move to the next path or begin synthesis. A multi-layer architecture is not one answer to synthesise — it is multiple independent answers, each needing its own verification. Do not synthesise until every active path has passed its own gate.

### Step 2.1 — Topology first

`listClusters` → `getClusterInfo` → `searchRepositories` → `getRepositoryInfo(summary)`

### Step 2.2 — Code-level search

Use the query plan in `references/exploration-queries.md` for the matched type.

**Before every call:**
```
Searching for: [exact thing]
Advancing goal by: [which blank in The One Test this fills]
If empty: [next attempt]
```

**After every call, ask one question:**
> Can I now complete all three lines of The One Test?
> — Line 1 needs a scalar, not a struct. If struct: find the transformation.
> — Line 2 needs the file:line of the producing function. If missing: find it.
> — Line 3 needs the client's function body read, confirming a network/protocol call. If not read: read it now. Do not infer from the client's name.
> — If no result for target at this client: check remaining access paths before concluding absent.

### Step 2.3 — Following the transformation

When a result is a struct or intermediate:
1. What function/library receives this struct?
2. `searchRepositories("<library name>")` — internal libraries are almost always indexed
3. `searchSymbols("<function name>")` → `getCode(...)` — read the transformation
4. Re-ask: can I now complete The One Test?

**DAL / shared-library checkpoint:** If an internal DAL, shared library, or framework is responsible for serialising the value into the final storage format, that library is almost certainly indexed. The claim "the library handles serialisation internally" is not a Line 1 completion — it is a description of where Line 1 is hiding. Run `searchRepositories("<library name>")` immediately. If indexed, read the serialisation function and complete Line 1 from its output. Only mark ❓ after attempting this.

When no result at a specific client:
1. Try 3 distinct search patterns
2. Check all other access paths found in Step 2.0
3. Only conclude absent after all access paths are exhausted — state what was searched

### Step 2.4 — Empty results

3 distinct patterns before marking ❓. Search all abstractions before concluding technology absence.

---

## The One Test Gate

**Before Phase 4, complete all three lines in writing:**

> **Line 1:** "The exact value that crosses the boundary is: `___`"
> **Line 2:** "I read the function at `[file:line]` that produces this scalar."
> **Line 3:** "I read the function body of `[client method]` at `[file:line]` and confirmed it issues a `[protocol]` call to `[technology]` — not an in-process operation."

If Line 1 contains a struct: gate fails.
If Line 3 was inferred rather than read from the client's function body: gate fails.
Return to Phase 2 with a specific next step.

Also confirm:
- At least one adjacent result was found and excluded (named with reason)
- If a negative result was encountered: state every access path that was searched

---

## Phase 3: Knowledge Context (optional)

Code shows WHAT. Knowledge tools show WHY. Use only when rationale questions arise.
Infer from code structure when unavailable. Label inferences ⚠️.

---

## Phase 4: Output

1. Answer first — the boundary value before tables or diagrams.
2. Confidence: ✅ confirmed from read code | ⚠️ inferred | ❓ not found after exhaustive search.
3. Name what was ruled out — adjacent results with reasons.
4. Surface gaps — what couldn't be confirmed and what was tried.

Persona calibration in `references/output-templates.md`.

---

## Anti-Pattern Reference

| What the agent does | The failure | Correct behaviour |
|---|---|---|
| Finds multiple access paths, verifies some but not all, then synthesises | Multi-layer architecture does not reduce the verification requirement. Each active path needs its own Line 1–3 before synthesis. | Apply the per-path rule (Step 2.0): complete The One Test for each path independently before combining results. |
| Claims "the library/DAL handles serialisation internally" and produces output | This is narrative closure — a satisfying architectural story that substitutes for actual Line 1 evidence. The serialisation library almost certainly exists in the index. | Run `searchRepositories("<library name>")` immediately. Read the serialisation function. Complete Line 1 from its output. |
| Claims a wrapper "routes to" the storage client without reading the wrapper's code | A client's name does not prove its internals. A cache named "Redis" may be in-process. A cache named "BigCache" may call Redis. Names lie; source code does not. | Read the function body of the client method (Line 3 of The One Test). Confirm it makes a network/protocol call to the target technology. |
| Reports a struct or composite key as the answer | A struct is an intermediate. It gets transformed into a scalar before crossing the storage boundary. | Find the function that transforms the struct into the scalar. Read it. Report its output. |
| Finds the call site for a wrapper and treats it as the storage boundary | A wrapper boundary is not the storage boundary. The client inside the wrapper makes the final call. | Follow the chain inside the wrapper to the actual storage client call. Read the function body (Line 3). |
| Concludes the entity is absent from a technology after searching one client | Another abstraction may use the same technology. One negative result ≠ technology absence. | Identify all abstractions for the technology (Step 2.0). Check the entity through each one. |
| Cannot complete The One Test but produces output anyway | The One Test exists precisely to prevent this. Plausible-looking output is not confirmed output. | If any line of The One Test is unfilled or inferred, return to Phase 2 with a specific next step. |
| Finds a key at the wrong layer and reports it as the answer | Different clients produce different values. Layer A ≠ Layer B. | Confirm which specific client the value flows to. Read that client's body to confirm the technology. |
| Assumes a library is unreadable without checking | Internal libraries are almost always indexed. | `searchRepositories("<library>")` before concluding unavailability. |
| Treats one empty search as ❓ | One failed pattern proves only that pattern failed. | 3 distinct patterns before marking ❓. |
