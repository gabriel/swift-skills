---
name: swiftui-components
description: >
  Reusable SwiftUI custom views and view modifiers from the SwiftUIPatterns library. Use when
  you need a drop-in SwiftUI component for scroll viewport geometry, deterministic preview text,
  or other layout utilities. Triggers on: "custom SwiftUI view", "view modifier", "SwiftUI component",
  "ScrollView size", "viewport", "GeometryReader in ScrollView", "carousel sizing",
  "lazy stack sizing", "lorem ipsum", "preview text", "deterministic text",
  "HipsterLorem", "ScrollViewport", "reusable SwiftUI", "SwiftUI utility".
---

# SwiftUI Components

## Overview

Drop-in custom views and view modifiers for common SwiftUI needs:

1. **ScrollViewport** (custom view) — Wraps `ScrollView` to capture viewport geometry before scroll takes control, injecting size and safe area insets via Environment so descendants can size relative to the container.
2. **HipsterLorem** (utility + Text extension) — Deterministic lorem ipsum generator with seeded RNG for stable previews and snapshot tests. Includes a `Text.hipsterLoremParagraphs` view modifier for direct use in SwiftUI.

## Components

### ScrollViewport

A custom `View` that wraps `ScrollView` to solve the problem of `GeometryReader` reporting content space instead of container space inside scroll views.

**When to use:**
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

### HipsterLorem

A utility enum with static methods and a `Text` extension for generating deterministic, hipster-flavored lorem ipsum.

**When to use:**
- Stable preview layouts that don't change between runs
- Snapshot tests that need reproducible text
- Stress-testing text truncation or long-form layouts

```swift
import SwiftUIPatterns

// Static API:
HipsterLorem.words(5, seed: 1)           // 5 deterministic words
HipsterLorem.sentence(seed: 2)           // One sentence
HipsterLorem.paragraphs(3, seed: 3)      // 3 paragraphs
HipsterLorem.approximately(1400, seed: 4) // ~1400 characters

// Text view extension:
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
