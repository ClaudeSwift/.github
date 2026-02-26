<div align="center">

# ClaudeSwift

**The Swift platform for Claude AI — SDK, tools, and apps for Apple developers.**

[![Swift 6.0+](https://img.shields.io/badge/Swift-6.0+-F05138.svg?style=flat&logo=swift&logoColor=white)](https://swift.org)
[![Platforms](https://img.shields.io/badge/Platforms-iOS%20%7C%20macOS%20%7C%20watchOS%20%7C%20tvOS%20%7C%20visionOS%20%7C%20Linux-007AFF.svg?style=flat)](https://developer.apple.com)
[![License](https://img.shields.io/badge/License-MIT-34D058.svg?style=flat)](https://github.com/ClaudeSwift/ClaudeSwiftSDK/blob/main/LICENSE)

</div>

---

## Overview

ClaudeSwift is an open-source organization building production-grade Swift tools for [Anthropic's Claude API](https://www.anthropic.com/claude). Our mission is to provide the most idiomatic, type-safe, and performant Claude integration for the Apple ecosystem and beyond.

**What we build:**

| Repository | Description | Status |
|------------|-------------|--------|
| [ClaudeSwiftSDK](https://github.com/ClaudeSwift/ClaudeSwiftSDK) | Core Swift SDK for the Claude API | Stable |
| [ClaudeMacOSMenuBar](https://github.com/ClaudeSwift/ClaudeMacOSMenuBar) | macOS menu bar app for AI text enhancement | Beta |
| [ClaudeBatchCLI](https://github.com/ClaudeSwift/ClaudeBatchCLI) | High-scale batch inference CLI | Beta |

---

## ClaudeSwiftSDK

The core SDK. Zero dependencies. Swift 6 strict concurrency. Cross-platform support for iOS, macOS, watchOS, tvOS, visionOS, and Linux.

```swift
import ClaudeSwift

let client = ClaudeClient(apiKey: "sk-ant-...")

let response = try await client.messages.create(
    model: .sonnet4_5,
    maxTokens: 1024,
    messages: [.user("Hello, Claude!")]
)
```

### Key Features

| Feature | Description |
|---------|-------------|
| Messages API | Complete coverage: create, stream, tool use, vision, batch |
| Prompt Caching | `cacheControl: .ephemeral` with cache hit/miss metadata |
| Streaming | `AsyncThrowingStream` with proper cancellation propagation |
| Extensibility | `ClaudeRequestInterceptor` and `ClaudeResponseObserver` protocols |
| Retry Policies | Exponential backoff with jitter, configurable per-client |
| Session Management | SwiftData-backed conversation persistence |
| MemoryKit | Vector store, RAG pipeline, knowledge base for retrieval-augmented generation |
| Test Coverage | 74 tests passing |

### Installation

```swift
// Package.swift
dependencies: [
    .package(url: "https://github.com/ClaudeSwift/ClaudeSwiftSDK.git", from: "1.0.0")
]
```

---

## ClaudeMacOSMenuBar

AI-powered text enhancement that lives in your menu bar. Built for developers, writers, and anyone who works with text.

**Workflow:** Copy text → select an enhancement → paste the improved version.

### Available Enhancements

- Grammar and Spelling
- Professional Tone
- Casual Tone
- Make Concise
- Expand and Detail
- Summarize
- Translate to English

### Quick Start

```bash
git clone https://github.com/ClaudeSwift/ClaudeMacOSMenuBar.git
cd ClaudeMacOSMenuBar && swift run ClaudeMacOSMenuBar
```

Built with SwiftUI `MenuBarExtra`, actor-based concurrency, and ClaudeSwiftSDK.

---

## ClaudeBatchCLI

Process thousands of prompts concurrently from the command line. Designed for high-scale batch inference workflows.

```bash
export ANTHROPIC_API_KEY=sk-ant-...

claude-batch -i prompts.jsonl -o results.jsonl -w 10 --verbose
```

### Features

| Feature | Description |
|---------|-------------|
| JSONL I/O | One prompt per line, one result per line |
| Concurrency | Configurable parallel workers with `-w` flag |
| Rate Limiting | Respects API limits with exponential backoff |
| Token Accounting | Input/output tokens, cache hits, latency per request |
| Metrics | Throughput, success rate, cost estimation on completion |
| Cross-Platform | Runs on macOS and Linux |

### Quick Start

```bash
git clone https://github.com/ClaudeSwift/ClaudeBatchCLI.git
cd ClaudeBatchCLI && swift build -c release
```

---

## Why ClaudeSwift?

| | ClaudeSwift | Python SDK | TypeScript SDK |
|---|-------------|------------|----------------|
| Language | Swift 6 | Python 3.8+ | TypeScript/Node |
| Concurrency | Actors + async/await | asyncio | Promises |
| Type Safety | Compile-time enums | Runtime dicts | Partial types |
| Dependencies | Zero | httpx, pydantic, etc. | node-fetch, etc. |
| Apple Platforms | Native support | No | No |
| Server-Side | macOS + Linux | Yes | Yes |

### Design Principles

1. **Idiomatic Swift** — Leverages Swift 6 features: actors, async/await, structured concurrency
2. **Type-Safe** — Enums for models, errors, and parameters; compile-time guarantees
3. **Zero Dependencies** — Pure Swift implementation, no external packages
4. **Production-Ready** — Comprehensive error handling, retry logic, and test coverage
5. **Cross-Platform** — iOS, macOS, watchOS, tvOS, visionOS, and Linux support

---

## Getting Started

### Step 1: Get an API Key

Sign up at [console.anthropic.com](https://console.anthropic.com) and create an API key.

### Step 2: Choose Your Entry Point

| Use Case | Recommended Tool |
|----------|------------------|
| Building an iOS/macOS app | [ClaudeSwiftSDK](https://github.com/ClaudeSwift/ClaudeSwiftSDK) |
| Daily productivity tool | [ClaudeMacOSMenuBar](https://github.com/ClaudeSwift/ClaudeMacOSMenuBar) |
| Batch processing at scale | [ClaudeBatchCLI](https://github.com/ClaudeSwift/ClaudeBatchCLI) |

### Step 3: Install and Use

```swift
// Package.swift
dependencies: [
    .package(url: "https://github.com/ClaudeSwift/ClaudeSwiftSDK.git", from: "1.0.0")
]
```

---

## Contributing

Contributions are welcome across all repositories. Each repo includes a `CONTRIBUTING.md` with setup instructions.

| Action | Link |
|--------|------|
| Report a bug | [Issue Tracker](https://github.com/ClaudeSwift/ClaudeSwiftSDK/issues/new?template=bug_report.md) |
| Request a feature | [Feature Requests](https://github.com/ClaudeSwift/ClaudeSwiftSDK/issues/new?template=feature_request.md) |
| Read the source | [ClaudeSwiftSDK Repository](https://github.com/ClaudeSwift/ClaudeSwiftSDK) |

**Security:** Report vulnerabilities via [GitHub Security Advisories](https://github.com/ClaudeSwift/ClaudeSwiftSDK/security/advisories/new). Do not open public issues for security concerns.

---

## Requirements

| Platform | Minimum Version |
|----------|-----------------|
| Swift | 6.0+ |
| iOS | 16.0+ |
| macOS | 13.0+ |
| watchOS | 9.0+ |
| tvOS | 16.0+ |
| visionOS | 1.0+ |
| Linux | Ubuntu 20.04+ |

---

## Community and Support

- **Issues:** [GitHub Issues](https://github.com/ClaudeSwift/ClaudeSwiftSDK/issues)
- **Discussions:** [GitHub Discussions](https://github.com/ClaudeSwift/ClaudeSwiftSDK/discussions)
- **Security:** security@claudeswift.org

---

## License

All ClaudeSwift projects are released under the MIT License. See individual repositories for details.

---

<div align="center">

**Built by developers, for developers.**

ClaudeSwift is not affiliated with Anthropic PBC. Claude and Anthropic are trademarks of Anthropic PBC.

</div>
