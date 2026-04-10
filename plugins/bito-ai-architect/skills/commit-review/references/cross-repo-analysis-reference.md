# Cross-Repo Impact Analysis Reference

This document provides detailed guidance on using BitoAIArchitect MCP for cross-repo impact analysis during commit review.

## Analysis Patterns by Change Type

### 1. API Endpoint Changes

When staged changes modify API endpoints (routes, controllers, handlers):

```
Step 1: Identify the endpoint
- Look for route definitions, path patterns, HTTP methods

Step 2: Find consumers
- searchCode(pattern: "/api/v1/users", repositories: [])
  → Find all repos that call this endpoint

Step 3: Check for breaking changes
- Parameter changes (required → optional, type changes)
- Response schema changes
- Status code changes
- Rate limiting or auth changes
```

### 2. Exported Function/Class Changes

When staged changes modify exported modules:

```
Step 1: Identify exports
- Look for `export`, `module.exports`, `public` declarations

Step 2: Find importers
- searchCode(pattern: "import.*from.*module-name", repositories: [])
- searchSymbols(pattern: "FunctionName", repositories: [])

Step 3: Verify compatibility
- Signature changes (parameters, return type)
- Behavioral changes (side effects, exceptions)
```

### 3. Database/Schema Changes

When staged changes modify database models or migrations:

```
Step 1: Identify affected tables/collections
- Look for model definitions, migration files

Step 2: Find consumers
- searchCode(pattern: "SELECT.*FROM.*table_name")
- searchCode(pattern: "table_name", filePattern: "*.model.*")

Step 3: Assess impact
- Column additions (usually safe)
- Column removals (breaking)
- Type changes (potentially breaking)
- Index changes (performance impact)
```

### 4. Shared Type/Interface Changes

When staged changes modify shared types or interfaces:

```
Step 1: Identify shared types
- Look for interfaces, types, DTOs in shared packages

Step 2: Find implementations
- searchSymbols(pattern: "InterfaceName", symbolKind: "interface")
- searchCode(pattern: "implements InterfaceName")

Step 3: Check compatibility
- Added required fields (breaking for consumers)
- Removed fields (breaking for producers)
- Type changes (potentially breaking)
```

### 5. Configuration Changes

When staged changes modify configuration or environment:

```
Step 1: Identify config changes
- Look for .env, config files, constants

Step 2: Find dependent services
- searchCode(pattern: "CONFIG_KEY_NAME")
- getRepositoryInfo with includeIncomingDependencies

Step 3: Coordinate deployment
- Config changes may require coordinated rollout
```

## BitoAIArchitect Tool Usage Examples

### Get Repository Dependencies

```javascript
// What services depend on THIS repo?
getRepositoryInfo({
  repositoryName: "my-service",
  includeIncomingDependencies: true,
  purposeType: "impact_analysis",
  purpose: "Finding services that will be affected by changes to my-service"
})

// What does THIS repo depend on?
getRepositoryInfo({
  repositoryName: "my-service",
  includeOutgoingDependencies: true,
  purposeType: "impact_analysis",
  purpose: "Understanding downstream dependencies that might cause issues"
})
```

### Search for Code Patterns Across Repos

```javascript
// Find all usages of a function
searchCode({
  pattern: "processPayment",
  repositories: [],  // empty = search all repos
  purposeType: "impact_analysis",
  purpose: "Finding all services that call processPayment function"
})

// Find API endpoint consumers
searchCode({
  pattern: "/api/v1/orders",
  caseSensitive: false,
  purposeType: "impact_analysis",
  purpose: "Finding all services that call the orders API"
})
```

### Find Symbol Definitions and Usages

```javascript
// Find function/class definitions
searchSymbols({
  pattern: "OrderService",
  symbolKind: "class",
  purposeType: "impact_analysis",
  purpose: "Finding OrderService implementations across repos"
})

// Find all methods starting with 'create'
searchSymbols({
  pattern: "create*",
  purposeType: "impact_analysis",
  purpose: "Finding create methods that might be affected"
})
```

### Understand Cluster Architecture

```javascript
// See how repos are grouped
listClusters({
  purposeType: "architecture_analysis",
  purpose: "Understanding service clusters to assess blast radius"
})

// Get details of a specific cluster
getClusterInfo({
  clusterId: "cluster_0",
  includeSummary: true,
  purposeType: "impact_analysis",
  purpose: "Understanding which services are tightly coupled"
})
```

### Compare Patterns Across Repos

```javascript
// Check a field across multiple repos
queryFieldAcrossRepositories({
  repositories: ["service-a", "service-b", "service-c"],
  fieldPath: "dependencies",
  purposeType: "comparative_analysis",
  purpose: "Comparing dependency patterns across services"
})
```

## Impact Classification Matrix

| Change Type | Local Impact | Cross-Repo Impact | Risk Level |
|-------------|--------------|-------------------|------------|
| Bug fix (internal) | Low | None | LOW |
| Bug fix (API behavior) | Low | High | HIGH |
| New feature (additive) | Medium | Low | LOW |
| New feature (API) | Medium | Medium | MEDIUM |
| Refactor (internal) | Low | None | LOW |
| Refactor (public API) | Low | High | HIGH |
| Schema migration (additive) | Low | Low | LOW |
| Schema migration (breaking) | Medium | High | CRITICAL |
| Config change | Low | Medium | MEDIUM |
| Dependency update | Medium | High | HIGH |

## Coordination Requirements

### HIGH Risk Changes
- Notify all dependent service teams BEFORE commit
- Coordinate deployment order
- Prepare rollback plan
- Consider feature flag

### MEDIUM Risk Changes
- Notify directly dependent teams
- Document in commit message
- Monitor after deployment

### LOW Risk Changes
- Standard code review
- Normal deployment process

## Common Cross-Repo Issues

### 1. Version Mismatch
Service A updates shared library, Service B still uses old version
→ Check `queryFieldAcrossRepositories` for version fields

### 2. API Contract Drift
Service A changes response format, Service B expects old format
→ Search for response parsing code in dependents

### 3. Database Contention
Multiple services modify same tables
→ Use `getRepositoryInfo` to find shared database dependencies

### 4. Event Schema Changes
Event publisher changes payload, subscribers break
→ Search for event handlers across repos

### 5. Configuration Conflicts
Shared config key means different things to different services
→ Search for config key usage across repos
