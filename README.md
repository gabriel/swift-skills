# Swift Skills

A Claude Code plugin that provides SwiftUI skills for navigation, components, dependency injection, and snapshot testing. Install the plugin and Claude gets actionable guidance with code examples when building SwiftUI apps.

## Install

```bash
claude plugins install github:gabriel/swift-skills
```

## Skills

### [swiftui-navigation](./skills/swiftui-navigation/SKILL.md)

Navigation and routing patterns using SwiftUI Routes — dual routing (URL-based deep linking + type-safe navigation), multi-package route registration, push/pop API.

### [swiftui-components](./skills/swiftui-components/SKILL.md)

Drop-in custom views and view modifiers:

- **ScrollViewport** — Captures viewport geometry before ScrollView takes control, so descendants can size relative to the container
- **HipsterLorem** — Deterministic lorem ipsum generator with seeded RNG for stable previews and snapshot tests

### [swiftui-dependencies](./skills/swiftui-dependencies/SKILL.md)

Dependency injection patterns with a decision matrix for choosing between four approaches:

- **Point-Free** (swift-dependencies) — Type-safe DI with `@Dependency` and `withDependencies` testing
- **FactoryKit** — Container-based DI with scopes, contexts, and `@Injected`
- **Microapps** — Zero-dependency struct-based injection with function types
- **Swift Service** — Concurrency-first DI with compiler-enforced Sendable/MainActor boundaries

### [swiftui-testing](./skills/swiftui-testing/SKILL.md)

SwiftUI snapshot testing with `assertRender` (pure SwiftUI, fast) and `assertSnapshot` (UIKit-hosted, accurate), device simulation, and async test patterns.

## Swift Library

This repo also ships a Swift package (`SwiftUIPatterns`) with the ScrollViewport and HipsterLorem components:

```swift
.package(url: "https://github.com/gabriel/swift-skills", from: "1.0.0")
```

## License

MIT
