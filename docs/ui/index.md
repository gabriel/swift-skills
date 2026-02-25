# UI Patterns

Subtle layout helpers and ergonomic components for building expressive SwiftUI interfaces.

## Installation

### 1. Add SwiftUIPatterns via Swift Package Manager

```swift
dependencies: [
    .package(url: "https://github.com/gabriel/swiftui-patterns", branch: "main")
],
targets: [
    .target(
        name: "MyFeature",
        dependencies: [
            .product(name: "SwiftUIPatterns", package: "swiftui-patterns")
        ]
    )
]
```

### 2. Or Copy the Component Source Files

If you only want specific patterns, copy the source files directly into your app target:

- `Sources/ScrollViewport.swift`
- `Sources/HipsterLorem.swift`

## Patterns

- [ScrollViewport](./ScrollViewport.md)
- [HipsterLorem](./HipsterLorem.md)
