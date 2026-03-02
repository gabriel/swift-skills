---
name: swiftui-testing
description: >
  SwiftUI snapshot testing patterns using SwiftUISnapshotTesting. Use when writing snapshot
  tests, choosing between assertRender and assertSnapshot, setting up visual regression
  testing, or configuring device simulation for SwiftUI views. Triggers on: "snapshot test",
  "SwiftUI test", "assertRender", "assertSnapshot", "visual regression", "snapshot testing",
  "SwiftUI snapshot", "device simulation test".
---

# SwiftUI Snapshot Testing Patterns

## Overview

Snapshot testing for SwiftUI views using `SwiftUISnapshotTesting` (wraps pointfreeco/swift-snapshot-testing). Two rendering strategies: pure SwiftUI (`assertRender`) and UIKit-hosted (`assertSnapshot`).

## When to Use Which

| | `assertRender` | `assertSnapshot` |
|---|---|---|
| **Rendering** | Pure SwiftUI (ImageRenderer) | UIKit-hosted (UIHostingController) |
| **Speed** | Faster | Slower |
| **Accuracy** | Good for pure SwiftUI | Better for complex hierarchies |
| **UIKit components** | No (NavigationStack, TabView) | Yes |
| **Cross-platform** | iOS + macOS | iOS + macOS |

**Rule of thumb:** Start with `assertRender`. Switch to `assertSnapshot` if the view uses NavigationStack, TabView, or other UIKit-backed components.

## Quick Start

```swift
import SwiftUISnapshotTesting
import Testing

@Test @MainActor
func testCard() throws {
    let view = CardView(title: "Hello")
    assertRender(view: view, device: .size(400, 400))
}

@Test @MainActor
func testScreen() throws {
    let view = NavigationStack { MyScreen() }
    assertSnapshot(view: view, device: .iOS(width: 375, height: 812))
}
```

## Device Options

- `.size(width, height)` — Custom size, any platform
- `.iOS(width, height)` — iOS device simulation
- `.macOS(width, height)` — macOS window simulation
- `.any` — Default size (400x1200 iOS, 1200x800 macOS)

## Installation

```swift
.package(url: "https://github.com/gabriel/swiftui-snapshot-testing", from: "0.1.12")
// Add "SwiftUISnapshotTesting" to test target dependencies
```

## References

Read `references/snapshot-testing.md` for async testing patterns, recording modes, and full API details.
