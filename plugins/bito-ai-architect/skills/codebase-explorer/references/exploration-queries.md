# Exploration Query Plans

Principle-based. No codebase-specific knowledge assumed. Any language, framework, architecture.

All calls: unique `purpose` per call.

**Before every call:** "Searching for: / Advancing goal by: / If empty:"
**After every call:** Can I complete The One Test? If not — what is missing and where do I look next?

---

## Developer Explorations

### Storage Key / Cache Key

The One Test for this type:
> "The exact string that the storage system receives is: `___`
> I read this at `[file:line]` (the function that produces the string),
> and it is passed to `[specific client type]` at `[file:line]`."

The test cannot be passed with a struct, a composite key object, or component fields.
It requires a scalar string confirmed from code.

**Step 0 — Map all access paths to the target technology (always first)**

The same storage technology may be accessed through multiple abstractions in this codebase.
Finding one client does not mean you have found all of them.

1. `getRepositoryInfo(repo, detailLevel: "summary")` — all storage/cache dependencies
2. `searchCode(pattern: "<technology keyword>", fileNamesOnly: true, repositories: [repo])` — find all related files; read imports to enumerate every client type AND every wrapper/adapter/abstraction
3. `getRepositoryInfo(repo, includeOutgoingDependencies: true)` — all external libraries

Output before Step 1: a list of every access path (direct clients + wrappers). You will check the target entity through each path.

**Step 1 — Find where the target entity's key is constructed**

`searchCode(pattern: "<entity keyword>", repositories: [repo])`

For each result: what client receives the key? Is that client on your access path list?
Trace each key to a call site. Note which layer each call site belongs to.

If a key flows to a wrapper rather than a direct storage client: this is not yet the storage boundary. The One Test is not yet passable. Proceed to Step 2.

**Step 2 — Follow every transformation until the scalar**

When a key construction returns a struct or is passed to a wrapper:
1. `searchRepositories("<library/wrapper name>")` — check if indexed (almost always is)
2. `searchSymbols("<transformation function>", repositories: [library_repo])` → `getCode(...)`
3. Read the function that converts the struct into the final string
4. Re-ask The One Test: is this the scalar the storage system receives?

Repeat until the scalar is found and the specific storage client call site is read.

**DAL / shared-library checkpoint:** If you find yourself writing "the library/DAL handles serialisation internally" — stop. That is not a Line 1 answer. It is a pointer to where Line 1 is hiding. Run `searchRepositories("<library name>")` now. Internal DALs and shared serialisation libraries are almost always indexed. Read the serialisation function body. Complete Line 1 from its output. Only accept ❓ after attempting this search.

**Step 3 — Verify all three lines of The One Test**

1. Confirm the scalar: the value produced in Step 2 is a string/number, not a struct.
2. Read the call site where the scalar is passed to the client.
3. **Read the client's function body.** `searchRepositories("<client library>")` → `getCode(client file, method start, method start+40)`. Confirm the method issues a network socket call, DB driver call, or protocol buffer write — not an in-process memory operation. This is Line 3. It cannot be inferred from the client's name.

If the client's function body shows an in-process operation (writing to a local map, calling another in-process library, etc.): this client is NOT the storage boundary. The value does not cross the target technology boundary through this path. Search other access paths.

If the client's function body confirms a network/protocol call to the target technology: Line 3 is complete. Proceed to the gate.

**Step 4 — Handle negative results**

If no key for the target entity is found at a given client:
1. Try 3 distinct patterns at that client
2. Check all other access paths from Step 0
3. If all paths exhausted: conclude absent, state every path searched

---

### System Overview

One Test equivalent: "The service's purpose, callers, and dependencies are: `___`
confirmed from live BitoAI data — not inferred from names."

1. `searchRepositories("<service name>")` — confirm repo
2. `getRepositoryInfo(repo, detailLevel: "summary")` — purpose, tech
3. `getRepositoryInfo(repo, includeIncomingDependencies: true, includeOutgoingDependencies: true, detailLevel: "standard")` — actual named lists
4. `getClusterInfo(clusterId)` — cluster context

---

### Call Path Trace

One Test equivalent: "The complete call sequence from entry to terminal is:
`[entry file:line]` → `[hop file:line]` → ... → `[terminal call file:line]`
each confirmed by reading the function body."

1. `searchRepositories("<service name>")` — confirm repo
2. `searchSymbols("<entry point>")` → `getCode(...)` — read entry; extract calls
3. For each call: `searchSymbols("<called name>")` → `getCode(...)` — read next hop
4. Repeat until every branch reaches a terminal (DB call, queue publish, external API, error return)

A trace that ends at "probably calls X" cannot pass The One Test. Each hop must be read.

---

### API Reference

One Test equivalent: "The exposed endpoints are: `[METHOD /path]` confirmed from route
registration code at `[file:line]`."

1. `searchRepositories("<service name>")` — confirm repo
2. `searchSymbols("<handler pattern>")` — find handlers
3. `searchCode(pattern: "<route registration pattern>")` — find registrations
4. `getCode(...)` — read registration to confirm path string

Handler name ≠ endpoint path. The path must be read from registration code.

---

### Data Model

One Test equivalent: "The fields of the `[entity]` type are: `[field list]`
confirmed by reading the type definition at `[file:line]`."

1. `searchSymbols("<entity name>", symbolKind: "type")` — find definitions
2. `getCode(repo, filePath, startLine, startLine+60)` — read type body
3. If multiple types share the name: identify which the question is about; read all candidates

---

### Runtime Value Tracing

One Test equivalent: "The runtime value of `[config key]` is: `___`
confirmed at `[file:line]` in `[config file]`."

1. `searchCode(pattern: "<config field name>")` — find read site
2. `getCode(...)` — read binding
3. `searchCode(pattern: "<field name>", filePattern: "*.yml\|*.yaml\|*.env\|*.json\|*.toml")` — find value

Config field name ≠ config value. The value must be read from the config file.

---

### Change Impact

One Test equivalent: "The callers of `[symbol]` are: `[named list with file:line]`
each confirmed by reading a call site."

1. `getRepositoryInfo(repo, includeIncomingDependencies: true, detailLevel: "full")` — caller repos
2. For each caller: `searchCode(pattern: "<exact symbol>", repositories: [caller_repo])` — confirm call site
3. `getFieldPath(repo, "outgoing_dependencies")` — downstream

A count is not a list. Caller names must be retrieved from actual call sites.

---

### Where to Change

One Test equivalent: "The touch points are: `[file:line list]`
each confirmed by reading the code."

1. `searchSymbols("<function or type>")` — find candidates
2. `getCode(...)` — confirm each candidate
3. `searchCode(pattern: "<related pattern>")` — find parallel touch points

---

### Entry Points

One Test equivalent: "The handler for `[request/event]` is at `[file:line]`
confirmed by reading the registration code."

1. `searchCode(pattern: "<route/event/trigger keyword>")` — find registration
2. `searchSymbols("<handler name>")` → `getCode(...)` — confirm correct handler

---

### Pattern Examples

One Test equivalent: "The implementation of `[pattern]` is the code at `[file:line]`
(full function body read)."

1. `searchCode(pattern: "<keyword>", fileNamesOnly: true)` — find candidates
2. `searchSymbols("<function>")` → `getCode(repo, file, start, start+80)` — read full body

---

## Engineering Manager Explorations

### Ownership Map
1. `listClusters()` + `getClusterInfo(clusterId, includeSummary: true)` for each
2. `listRepositories(includeSummary: true)` — naming signals
3. `getRepositoryInfo(ambiguous_repos, detailLevel: "standard")` — metadata

### Coupling Analysis
1. `queryFieldAcrossRepositories(repositories: [all_in_scope], fieldPath: "incoming_dependencies")` — rank
2. `getRepositoryInfo(top_N, includeIncomingDependencies: true, detailLevel: "full")` — actual caller names

---

## Product Manager Explorations

### Feature-to-Service Map
1. `searchRepositories("<feature keyword>")` → `getRepositoryInfo(candidates, detailLevel: "summary")`
2. `getClusterInfo(clusterId)` + `getRepositoryInfo(repo, includeOutgoingDependencies: true)`

---

## VP of Engineering Explorations

### Deployment Risk Map
1. `queryFieldAcrossRepositories(repositories: [all], fieldPath: "incoming_dependencies")` — rank
2. `getRepositoryInfo(top_N, includeIncomingDependencies: true, detailLevel: "full")` — caller names
3. `listClusters()` + `getClusterInfo` for cross-cluster deps

---

## CTO Explorations

### Architecture Overview
1. `listClusters()` + `getClusterInfo(clusterId, includeSummary: true)` for each
2. `queryFieldAcrossRepositories(repositories: [boundary_services], fieldPath: "tech_stack")`
3. `getRepositoryInfo(boundary_services, includeOutgoingDependencies: true)`

### Architectural Risk Register
1. `listClusters()` + `getClusterInfo(clusterId)`
2. `queryFieldAcrossRepositories(repositories: [all], fieldPath: "incoming_dependencies")` — coupling
3. `queryFieldAcrossRepositories(repositories: [all], fieldPath: "tech_stack")` — diversity
