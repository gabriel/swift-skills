# Design: SwiftUI Patterns Claude Code Plugin

**Date:** 2026-03-01
**Status:** Approved

## Goal

Convert the SwiftUI Patterns repository into a Claude Code plugin so the documented patterns are available as invokable skills during coding sessions. The existing Swift library, tests, and VitePress docs remain untouched.

## Plugin Structure

### Manifest

`.claude-plugin/plugin.json` — Plugin named `swiftui-patterns`, installable via `claude plugins install github:gabriel/swiftui-patterns`.

### Skills (4 category-level)

Each skill has a lean SKILL.md (~50-100 lines) with frontmatter triggers and a decision framework, plus a `references/` subdirectory containing the full pattern documentation.

#### 1. `skills/swiftui-navigation/`

- **Triggers:** Navigation, routing, deep linking, NavigationStack, multi-package navigation, SwiftUI Routes
- **SKILL.md:** When to use SwiftUI Routes, basic setup pattern, URL vs Type routing decision
- **references/swiftui-routes.md** — Full SwiftUI Routes documentation (dual routing, multi-package registration, push/pop API)

#### 2. `skills/swiftui-ui/`

- **Triggers:** ScrollView geometry, viewport sizing, lazy stacks, carousel sizing, lorem ipsum, preview text, deterministic text, HipsterLorem, ScrollViewport
- **SKILL.md:** Quick guide to ScrollViewport and HipsterLorem with when-to-use guidance
- **references/scroll-viewport.md** — ScrollViewport documentation (environment injection, viewport depth, GeometryReader workaround)
- **references/hipster-lorem.md** — HipsterLorem documentation (seeded RNG, API table, preview/snapshot usage)

#### 3. `skills/swiftui-dependencies/`

- **Triggers:** Dependency injection, DI, service container, testing dependencies, mock dependencies, @Dependency, @Injected, Factory, Point-Free dependencies, microapps
- **SKILL.md:** Decision matrix for choosing between 4 DI approaches with trade-off table, then directs to appropriate reference
- **references/point-free-dependencies.md** — Point-Free swift-dependencies (DependencyKey, withDependencies, liveValue/testValue)
- **references/factory.md** — FactoryKit (Container, scopes, @Injected/@InjectedObservable, contexts)
- **references/microapps.md** — Microapps architecture (struct Dependencies, function types, no third-party package)
- **references/swift-service.md** — Swift Service (@Service/@MainService, task-local environments, Swift 6.2+)

#### 4. `skills/swiftui-testing/`

- **Triggers:** Snapshot testing, SwiftUI testing, assertRender, assertSnapshot, visual regression, SwiftUI snapshot
- **SKILL.md:** assertRender vs assertSnapshot decision, device options, setup
- **references/snapshot-testing.md** — Full snapshot testing documentation (Swift Testing integration, async support, device simulation)

## Directory Tree

```
swiftui-patterns/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── swiftui-navigation/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── swiftui-routes.md
│   ├── swiftui-ui/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── scroll-viewport.md
│   │       └── hipster-lorem.md
│   ├── swiftui-dependencies/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── point-free-dependencies.md
│   │       ├── factory.md
│   │       ├── microapps.md
│   │       └── swift-service.md
│   └── swiftui-testing/
│       ├── SKILL.md
│       └── references/
│           └── snapshot-testing.md
├── Sources/          # unchanged
├── Tests/            # unchanged
├── docs/             # unchanged
└── ...
```

## Design Decisions

1. **4 skills, not 7:** Grouping by category keeps the plugin focused. Claude reads one SKILL.md, gets the decision framework, then reads the relevant reference file. This avoids skill sprawl.

2. **Lean SKILL.md + references:** The SKILL.md stays under 100 lines so it loads fast. Detailed code examples live in references/ and are loaded on-demand.

3. **Copies, not symlinks:** Reference files are independent copies adapted from docs/ content. This avoids filesystem symlink issues across platforms and lets us tailor the content for AI consumption (stripping VitePress-specific formatting).

4. **Additive only:** No existing files are modified or removed.

## SKILL.md Content Pattern

Each SKILL.md follows this structure:

```
---
name: skill-name
description: Trigger description with keywords
---

# Title

## Overview
Brief description of what this skill covers.

## When to Use
Bullet points of trigger scenarios.

## Quick Reference
Minimal code example or decision table.

## References
Links to reference files with Read tool instructions.
```
