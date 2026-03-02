---
name: swiftui-ui
description: >
  SwiftUI UI component patterns including ScrollViewport and HipsterLorem. Use when
  working with ScrollView geometry, viewport sizing inside lazy stacks, carousel layouts,
  or needing deterministic lorem ipsum text for previews and snapshot tests. Triggers on:
  "ScrollView size", "viewport", "GeometryReader in ScrollView", "carousel sizing",
  "lazy stack sizing", "lorem ipsum", "preview text", "deterministic text",
  "HipsterLorem", "ScrollViewport".
---

# SwiftUI UI Patterns

## Overview

Two complementary utilities for SwiftUI layout and previews:

1. **ScrollViewport** — Captures viewport geometry before ScrollView takes control, injecting it via Environment so descendants can size relative to the container.
2. **HipsterLorem** — Deterministic lorem ipsum generator with seeded RNG for stable previews and snapshot tests.

## ScrollViewport — When to Use

- You need the scroll container's size inside lazily loaded content (cards, carousels, grids)
- `GeometryReader` inside a `ScrollView` reports content space, not container space
- Nested scroll views — only the outermost pays the measurement cost

```swift
import SwiftUIPatterns

ScrollViewport {
    ScrollView(.horizontal) {
        LazyHStack {
            ForEach(items) { item in
                CardView(item: item)
                    .frame(width: viewport.size.width * 0.8)
            }
        }
    }
}

// Read in any descendant:
@Environment(\.viewport) private var viewport
```

## HipsterLorem — When to Use

- Stable preview layouts that don't change between runs
- Snapshot tests that need reproducible text
- Stress-testing text truncation or long-form layouts

```swift
import SwiftUIPatterns

// Quick API:
HipsterLorem.words(5, seed: 1)           // 5 deterministic words
HipsterLorem.sentence(seed: 2)           // One sentence
HipsterLorem.paragraphs(3, seed: 3)      // 3 paragraphs
HipsterLorem.approximately(1400, seed: 4) // ~1400 characters
Text.hipsterLoremParagraphs(2, seed: 5)  // SwiftUI Text view
```

## Installation

Add to `Package.swift`:
```swift
.package(url: "https://github.com/gabriel/swiftui-patterns", from: "1.0.0")
// Then add "SwiftUIPatterns" to your target dependencies
```

## References

- Read `references/scroll-viewport.md` for ScrollViewport details, nesting behavior, and tips
- Read `references/hipster-lorem.md` for full API table, preview examples, and snapshot testing patterns
