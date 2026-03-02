---
name: swiftui-navigation
description: >
  SwiftUI navigation patterns using SwiftUI Routes. Use when implementing navigation,
  routing, deep linking, NavigationStack path management, or multi-package navigation
  in SwiftUI apps. Triggers on: "navigation", "routing", "deep link", "NavigationStack",
  "push/pop", "SwiftUI Routes", "multi-package navigation".
---

# SwiftUI Navigation Patterns

## Overview

Guidance for implementing navigation in SwiftUI apps using the SwiftUI Routes package, which provides dual routing (URL-based for deep linking + Type-based for type safety) with multi-package support.

## When to Use

- Building navigation with `NavigationStack` that needs deep linking support
- Multi-package apps where features register their own routes independently
- You need both URL-based (loosely coupled) and Type-based (strongly coupled) navigation
- Replacing manual `navigationDestination` registrations with a centralized routing system

## Quick Start

```swift
import SwiftUIRoutes

// 1. Create and register routes
let routes = Routes()
routes.register(path: "/item") { url in ItemView(id: url.params["id"] ?? "") }
routes.register(type: Item.self) { item in ItemView(item: item) }

// 2. Attach to NavigationStack
NavigationStack(path: $routes.path) {
    ContentView()
        .routesDestination(routes)
}

// 3. Navigate from anywhere
@Environment(Routes.self) var routes
routes.push("/item", params: ["id": "42"])  // URL-based
routes.push(Item(id: "42"))                  // Type-based
routes.pop()
```

## Decision: URL vs Type Routes

| Factor | URL Routes | Type Routes |
|--------|-----------|-------------|
| Coupling | Loose — string paths | Tight — concrete types |
| Deep linking | Yes | No |
| Compile safety | Runtime | Compile-time |
| Cross-package | Easy (no shared types) | Needs shared type module |
| Best for | Deep links, analytics | Internal navigation |

**Recommendation:** Use both. URL routes for external entry points, type routes for in-app navigation.

## References

Read `references/swiftui-routes.md` for complete API documentation, multi-package registration patterns, and full example code.
