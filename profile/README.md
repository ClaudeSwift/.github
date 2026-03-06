<div align="center">

# ClaudeSwift

**The Swift platform for Claude AI. SDK + agent framework for production apps.**

[![Swift 6.0+](https://img.shields.io/badge/Swift-6.0+-F05138.svg?style=flat&logo=swift&logoColor=white)](https://swift.org)
[![Platforms](https://img.shields.io/badge/Platforms-iOS%20%7C%20macOS%20%7C%20watchOS%20%7C%20tvOS%20%7C%20visionOS-007AFF.svg?style=flat)](https://developer.apple.com)
[![License](https://img.shields.io/badge/License-MIT-34D058.svg?style=flat)](https://github.com/ClaudeSwift/ClaudeSwiftSDK/blob/main/LICENSE)

</div>

---

## What This Is

**[ClaudeSwiftSDK](https://github.com/ClaudeSwift/ClaudeSwiftSDK)** is a single Swift package that ships four libraries:

| Library | What it does |
|---------|-------------|
| **ClaudeSwift** | Full Claude API — messages, streaming, vision, tools, sessions, context management |
| **MemoryKit** | RAG pipeline, vector search, knowledge bases |
| **AgentKit** | Agent framework — ReAct, plan-and-execute, multi-agent, tool system |
| **AgentKitUI** | SwiftUI components — chat views, activity timelines, tool inspectors |

Zero external dependencies. Swift 6 strict concurrency. Every agent is an actor.

**There is no other Swift agent framework.** LangGraph, CrewAI, Claude Agent SDK — all Python or TypeScript. This is the only option for native Apple development.

---

## What You Can Build

### AI coding assistant for macOS

```swift
let agent = CodeAgent(client: client, workingDirectory: "~/Projects/MyApp")
let result = try await agent.run("Find the failing test, fix the bug, verify the build passes")
```

CodeAgent comes pre-wired with file system, shell, and code execution tools. It reads files, runs `swift build`, edits code, and iterates until the job is done.

### Customer support agent for iOS

```swift
let supportAgent = ReActAgent(
    client: client,
    tools: [
        orderLookupTool,
        refundTool,
        escalationTool,
        knowledgeBaseTool
    ],
    configuration: AgentConfiguration(
        model: .sonnet4_5,
        systemPrompt: "You are a support agent for Acme Corp. Look up orders, process refunds up to $50, escalate anything else.",
        maxIterations: 15
    )
)

// Drop-in SwiftUI chat interface
struct SupportView: View {
    var body: some View {
        AgentChatView(agent: supportAgent)
    }
}
```

### Research pipeline with multiple specialists

```swift
let supervisor = Supervisor(
    client: client,
    agents: [
        ReActAgent(name: "researcher", client: client, tools: [searchTool, webScrapeTool]),
        ReActAgent(name: "analyst", client: client, tools: [calculatorTool, chartTool]),
        ReActAgent(name: "writer", client: client, tools: [fileSystemTool]),
    ],
    model: .opus4_6
)

let result = try await supervisor.run(
    task: "Research Q4 revenue trends for the top 5 SaaS companies, analyze growth patterns, write a summary report"
)
```

The supervisor delegates subtasks to the right agent, aggregates results, and synthesizes a final output.

### RAG-powered knowledge assistant

```swift
let store = VectorStore()
let kb = KnowledgeBase(store: store)

// Ingest your docs
for doc in companyDocs {
    try await kb.addDocument(doc.text, category: doc.category)
}

// Agent with long-term memory
let bridge = AgentMemoryBridge(knowledgeBase: kb)
let context = try await bridge.retrieveContext(for: userQuestion)

let agent = ReActAgent(
    client: client,
    tools: [searchTool, calculatorTool],
    configuration: AgentConfiguration(
        systemPrompt: "You are a knowledge assistant.\n\n\(context)"
    )
)
```

### visionOS spatial AI assistant

```swift
// Works on every Apple platform including visionOS
let agent = ReActAgent(client: client, tools: spatialTools)

struct ImmersiveAssistant: View {
    @State var vm = AgentViewModel()

    var body: some View {
        VStack {
            AgentActivityView(viewModel: vm)
            Button("Analyze Scene") {
                vm.run(agent: agent, input: "Describe what's in the user's environment")
            }
        }
    }
}
```

---

## Production Capabilities

This isn't a toy wrapper. It's infrastructure for shipping real apps.

**SDK layer:**
- Automatic retry with exponential backoff and jitter (429, 5xx)
- Keychain credential storage for production key management
- Request interceptors and response observers for logging, metrics, auth
- SwiftData session persistence with import/export
- Context window management with automatic trimming strategies
- Prompt caching support (`cacheControl: .ephemeral`)

**Agent layer:**
- Configurable stop conditions: max iterations, max tokens, keyword triggers, custom predicates
- Full tool call audit trail with timing data (ToolCallRecord)
- AgentSnapshot for serializing agent state to JSON
- Working memory (key-value facts + scratchpad) per agent
- MemoryKit bridge for persistent knowledge across sessions

**Multi-agent layer:**
- AgentTeam: parallel execution (`runAll`) and sequential pipelines
- Supervisor: LLM-orchestrated delegation with automatic tool routing
- Handoff protocol for agent-to-agent transfers with context

**UI layer:**
- `@Observable` AgentViewModel — bind to any SwiftUI view
- AgentChatView: full chat interface with activity panel in one line
- AgentActivityView: live event timeline with icons, durations, token counts
- ToolCallView: expandable cards with input/output inspection

**Testing:**
- MockURLProtocol for deterministic API testing without network calls
- 116 tests across the full stack
- Every public type is `Sendable` — no concurrency warnings in Swift 6 strict mode

---

## Install

```swift
// Package.swift
dependencies: [
    .package(url: "https://github.com/ClaudeSwift/ClaudeSwiftSDK.git", from: "1.0.0")
]

// Import what you need
.target(name: "MyApp", dependencies: [
    "ClaudeSwift",    // SDK only
    "MemoryKit",      // + RAG and vector search
    "AgentKit",       // + Agent framework
    "AgentKitUI",     // + SwiftUI components
])
```

---

## Requirements

| | Minimum |
|---|---------|
| Swift | 6.0+ |
| iOS | 16.0+ (UI: 17.0+) |
| macOS | 13.0+ (UI: 14.0+) |
| watchOS | 9.0+ |
| tvOS | 16.0+ |
| visionOS | 1.0+ |

---

## Contributing

[Issue tracker](https://github.com/ClaudeSwift/ClaudeSwiftSDK/issues) &middot; [Source code](https://github.com/ClaudeSwift/ClaudeSwiftSDK) &middot; Security: security@claudeswift.org

MIT License. Not affiliated with Anthropic PBC.
