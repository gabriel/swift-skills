# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A Swift Package Manager library of reusable SwiftUI patterns and components, paired with a VitePress documentation site. The library ships two modules: `ScrollViewport` (environment-based viewport geometry injection for scroll views) and `HipsterLorem` (deterministic lorem ipsum generator for previews).

## Commands

### Swift

```bash
swift build                          # Build the library
swift test                           # Run all tests
swift test --filter ScrollViewportTests  # Run a single test suite
```

### Documentation (VitePress)

```bash
npm install                          # Install docs dependencies (first time)
npm run docs:dev                     # Local dev server with hot reload
npm run docs:build                   # Build static site → docs/.vitepress/dist
npm run docs:preview                 # Preview built site locally
```

## Architecture

**Dual-stack project:**
- `Sources/` — Swift library code (target: `SwiftUIPatterns`)
- `Tests/` — Snapshot tests using `SwiftUISnapshotTesting` and Swift Testing (`@Suite`, `@Test` macros)
- `docs/` — VitePress site with pattern guides organized into Navigation, UI, Dependencies, and Testing categories

**Key patterns in the library:**
- `ScrollViewport` uses SwiftUI `Environment` to inject a `Viewport` struct (size + safe area insets) before `ScrollView` takes control of geometry. Tracks nesting depth to avoid re-injection in nested scrolls.
- `HipsterLorem` uses a seeded RNG for deterministic, reproducible preview text.

## Code Style

- SwiftFormat is configured via `.swiftformat` — Swift 6.2, 4-space indent, 150 char max width, LF line breaks
- `before-first` wrapping for arguments, parameters, and collections
- `testable-first` import grouping
- VS Code auto-formats Swift on save

## CI/CD

GitHub Actions deploys the VitePress docs to GitHub Pages on push to `main`. The Swift package itself has no CI pipeline — tests run locally.
