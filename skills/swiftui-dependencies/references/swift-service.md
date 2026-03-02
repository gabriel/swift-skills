# Swift Service - Concurrency-First Dependency Injection

Swift Service
[github.com/nslogmeng/swift-service](https://github.com/nslogmeng/swift-service)
is a lightweight dependency injection framework focused on modern Swift concurrency. It provides compiler-enforced APIs for `Sendable` and `MainActor` services, plus task-local environments for test isolation.

How it Works: You register services in `ServiceEnv.current`, then resolve them with property wrappers such as `@Service` and `@MainService` (or provider variants for scope-driven behavior). For tests and previews, you can switch environments with `@TaskLocal` so dependency graphs stay isolated across async tasks.

## Basic Usage

### 1. Installation

```swift
dependencies: [
    .package(url: "https://github.com/nslogmeng/swift-service", .upToNextMajor(from: "1.0.0"))
],
targets: [
    .target(
        name: "MyFeature",
        dependencies: [
            .product(name: "Service", package: "swift-service")
        ]
    )
]
```

### 2. Register Services

```swift
import Service

protocol APIClient: Sendable {
    func fetchItems() async throws -> [String]
}

struct LiveAPIClient: APIClient {
    func fetchItems() async throws -> [String] {
        ["One", "Two", "Three"]
    }
}

@MainActor
@Observable
final class ItemsViewModel {
    var items: [String] = []

    func refresh() async {
        // MainActor service can depend on UI concerns.
    }
}

func registerServices() {
    ServiceEnv.current.register(APIClient.self) {
        LiveAPIClient()
    }

    ServiceEnv.current.registerMain(ItemsViewModel.self) {
        ItemsViewModel()
    }
}
```

### 3. Inject in Features

```swift
import Service
import SwiftUI

struct ItemsRepository {
    @Service var apiClient: APIClient

    func load() async throws -> [String] {
        try await apiClient.fetchItems()
    }
}

@MainActor
struct ItemsScreen: View {
    @MainService var viewModel: ItemsViewModel
    @Provider var apiClient: APIClient

    var body: some View {
        List(viewModel.items, id: \.self) { item in
            Text(item)
        }
    }
}
```

### 4. Isolate Test Environments

```swift
import Service
import Testing

struct MockAPIClient: APIClient {
    func fetchItems() async throws -> [String] { ["Mock"] }
}

@Test
func repositoryUsesTestEnvironment() async throws {
    await ServiceEnv.$current.withValue(.test) {
        ServiceEnv.current.resetAll()
        ServiceEnv.current.register(APIClient.self) { MockAPIClient() }

        let repository = ItemsRepository()
        let items = try? await repository.load()

        #expect(items == ["Mock"])
    }
}
```

## Why It Is Good

- Compiler-enforced concurrency model for both `Sendable` and `MainActor` registrations.
- Task-local environment switching makes parallel tests safer than a single mutable global container.
- Simple mental model for teams already familiar with register/resolve DI patterns.
- Multiple scopes (`singleton`, `transient`, `graph`, custom) cover common lifecycle needs.

## Where It Can Be Bad Fit

- Modern-platform requirement is strict: current package settings are iOS 18+, macOS 15+, tvOS 18+, watchOS 11+, and visionOS 2+.
- Swift 6.2 and strict concurrency expectations may be too aggressive for legacy codebases.
- Property-wrapper DI can hide dependency graphs if teams do not document registrations clearly.
- Smaller ecosystem than older DI options, so examples and community integrations may be fewer.

## When to Use It

- You are already on Swift 6 concurrency and want compiler-checked DI boundaries.
- You need task-local test isolation with minimal runtime overhead.
- You want a lightweight container without extra third-party dependencies.

## When to Avoid It

- You must support iOS 17 or earlier.
- Your team prefers explicit initializer injection everywhere for readability and auditability.
- You need a mature ecosystem with many extensions and long-term production history.
