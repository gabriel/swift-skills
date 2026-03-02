---
name: swiftui-dependencies
description: >
  Dependency injection patterns for SwiftUI apps. Use when implementing DI, choosing a
  DI approach, setting up service containers, creating mock dependencies for testing, or
  working with @Dependency, @Injected, Factory, or microapps patterns. Triggers on:
  "dependency injection", "DI", "service container", "mock dependencies", "test isolation",
  "@Dependency", "@Injected", "Factory", "Point-Free dependencies", "microapps architecture",
  "swift-service", "dependency management".
---

# SwiftUI Dependency Injection Patterns

## Overview

Four proven DI approaches for SwiftUI apps, each with different trade-offs. Choose based on your project's needs.

## Decision Matrix

| | Point-Free | FactoryKit | Microapps | Swift Service |
|---|---|---|---|---|
| **Third-party dep** | Yes | Yes | No | Yes |
| **Min iOS** | 13+ | 15+ | Any | 18+ |
| **Swift version** | 5.9+ | 5.9+ | Any | 6.2+ |
| **Test isolation** | `withDependencies` | `onTest {}` | Init injection | `@TaskLocal` |
| **Concurrency** | Manual | Manual | Manual | Compiler-enforced |
| **Learning curve** | Medium | Low | Low | Medium |
| **Ecosystem** | Large (TCA) | Large | None needed | Small |

## Quick Recommendations

- **Default choice:** Point-Free `swift-dependencies` — best balance of type safety, testability, and ecosystem
- **Already using Factory:** Stick with FactoryKit — scopes and contexts are powerful
- **Zero dependencies:** Microapps — struct Dependencies with function types, no packages needed
- **Swift 6.2+ / strict concurrency:** Swift Service — compiler-enforced Sendable/MainActor boundaries

## Pattern Summaries

### Point-Free (swift-dependencies)
```swift
@Dependency(\.networkClient) var networkClient
// Test: withDependencies { $0.networkClient = mock } operation: { ... }
```

### FactoryKit
```swift
@Injected(\.networkService) var networkService  // from Container extension
// Scopes: .singleton, .session, .cached, .shared, unique (default)
```

### Microapps
```swift
struct Dependencies {
    var search: (String) async throws -> [String]
}
let vm = SearchViewModel(dependencies: .mock)
```

### Swift Service
```swift
@Service var apiClient: APIClient
// Test: ServiceEnv.$current.withValue(.test) { ... }
```

## References

Read the reference file for your chosen approach:
- `references/point-free-dependencies.md` — Full Point-Free setup, view models, testing, advanced patterns
- `references/factory.md` — Full FactoryKit setup, scopes, contexts, debugging
- `references/microapps.md` — Full microapps architecture, app container, mock patterns
- `references/swift-service.md` — Full Swift Service setup, concurrency model, trade-offs
