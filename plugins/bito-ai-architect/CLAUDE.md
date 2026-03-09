# BitoAIArchitect Guidelines

## When to Use BitoAIArchitect

ALWAYS call BitoAIArchitect tools FIRST for ANY task involving:
- Code generation, modification, or refactoring
- Understanding repositories, services, or architecture
- Searching for code patterns, symbols, or implementations
- Analyzing dependencies or impact of changes
- Bug fixing, debugging, or production triage
- Writing tests or documentation
- Security audits or compliance checks

## Tool Selection Guide

| Task | Tools to Use |
|------|-------------|
| Generate/modify code | `searchSymbols` → `getCode` → `getRepositoryInfo` |
| Understand a repo | `searchRepositories` → `getRepositoryInfo` (detailLevel: "standard") |
| Find code patterns | `searchCode` with `filePattern` filter |
| Analyze dependencies | `getRepositoryInfo` (includeIncomingDependencies/includeOutgoingDependencies) |
| Cross-repo comparison | `queryFieldAcrossRepositories` |
| Discover architecture | `listClusters` → `getClusterInfo` |
| Find symbol definitions | `searchSymbols` with `symbolKind` filter |
| Read source code | `searchCode`/`searchSymbols` → `getCode` |

## Important Rules

1. **Always search before generating** — Check existing patterns, conventions, and implementations before writing new code.
2. **Use the right detail level** — Start with `summary`, escalate to `standard` or `full` only when needed.
3. **Filter dependencies by type** — Use `edge.type` field to isolate specific dependency categories.
4. **Respect repository naming** — Names are case-sensitive. Use `listRepositories` or `searchRepositories` to discover exact names.
5. **Paginate large results** — Use `arrayLimits` and `arraySlice` for large datasets.
6. **Combine tools efficiently** — Chain tool calls (e.g., search → get details → get code) for comprehensive answers.

## purposeType Reference

Always set `purposeType` to match the user's high-level task:
`codebase_understanding`, `onboarding`, `architecture_analysis`, `dependency_analysis`,
`impact_analysis`, `code_search`, `comparative_analysis`, `code_generation`,
`code_modification`, `refactoring`, `test_writing`, `code_review`, `bug_fixing`,
`production_triage`, `debugging`, `error_analysis`, `documentation`,
`diagram_generation`, `specification_generation`, `technical_design`, `planning`,
`migration`, `upgrade`, `security_audit`, `compliance_check`, `other`
