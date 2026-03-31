# Claude Code Sourcemap Learning Notebook

[![GitHub](https://img.shields.io/badge/GitHub-dadiaomengmeimei-blue?logo=github)](https://github.com/dadiaomengmeimei/claude-code-sourcemap-learning-notebook)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> 🌐 [中文版 / Chinese Version](README_CN.md)

> A deep dive into the internal architecture of Anthropic's Claude Code CLI tool — dissecting 500K+ lines of TypeScript code layer by layer, from entry points to tool systems, permission security, query loops, multi-agent coordination, MCP extensions, Voice mode, and the Buddy system.

## About This Project

[Claude Code](https://docs.anthropic.com/en/docs/claude-code) is Anthropic's AI programming assistant CLI tool that allows developers to interact with Claude AI directly from the terminal to perform software engineering tasks such as code editing, file searching, shell commands, and code review.

This repository is a set of **structured learning notes** in Markdown format, providing an in-depth analysis of Claude Code's core source code. Ideal for:

- **AI Tool Researchers** — Understanding the architecture of production-grade AI Agent systems
- **Security Researchers** — Studying permission control and security mechanisms in AI tools
- **Software Architects** — Learning engineering practices in large-scale TypeScript projects
- **Agent Developers** — Understanding design patterns for multi-agent collaboration and tool systems

## What Makes This Unique

This is not just a code walkthrough — it's an **architectural analysis** that extracts transferable engineering patterns:

| Feature | Description |
|---------|-------------|
| 🔍 **End-to-End Request Tracing** | Follow a real request through the entire query loop — from user input to API call to tool execution to response (Ch.04) |
| ⚖️ **Design Decision Analysis** | Why read-write locks over Actor model or DAG scheduling? Each major design choice is compared with alternatives (Ch.04) |
| 🧩 **Transferable Patterns** | 11 design patterns extracted from the codebase that you can apply to your own Agent systems (Ch.04, Ch.05) |
| 🎯 **Complete Orchestration Walkthrough** | A full Coordinator orchestration trace showing task decomposition, worker assignment, and result synthesis (Ch.05) |
| 🔒 **Security Deep Dive** | 5-layer defense-in-depth analysis with real bypass scenarios and safety floor guarantees (Ch.03) |
| 📊 **Worker Communication Taxonomy** | Three communication mechanisms (task-notification, SendMessage, Scratchpad) compared with timing guarantees (Ch.05) |

## Claude Code Technical Overview

| Category | Details |
|----------|---------|
| **Language** | TypeScript (strict mode) |
| **Runtime** | Bun |
| **Terminal UI** | React + Ink |
| **CLI Parsing** | Commander.js |
| **Schema Validation** | Zod v4 |
| **Code Search** | ripgrep |
| **Extension Protocol** | MCP (Model Context Protocol) |
| **API** | Anthropic SDK |
| **Codebase Size** | ~1,900 files, 512,000+ lines of code |

## Learning Roadmap

```
                    ┌─────────────────┐
                    │  00 Index       │
                    └────────┬────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
     ┌────────▼───────┐ ┌───▼────────┐ ┌──▼──────────┐
     │ 01 Architecture│ │ 02 Tools   │ │ 03 Security  │
     │ (Entry+State)  │ │ (Core)     │ │ (Defense)    │
     └────────┬───────┘ └───┬────────┘ └──┬──────────┘
              │              │              │
              └──────────────┼──────────────┘
                             │
                    ┌────────▼────────┐
                    │ 04 Query Loop   │
                    │ (System Heart)  │
                    └────────┬────────┘
                             │
                    ┌────────┼────────┐
                    │                 │
           ┌───────▼──────┐ ┌────────▼───────┐
           │ 05 Multi-    │ │ 06 MCP/Skills  │
           │ Agent+Coord  │ │ (Extensions)   │
           └──────┬───────┘ └───────┬────────┘
                  │                 │
                  └────────┬────────┘
                           │
                  ┌────────▼────────┐
                  │ 07 Prompt       │
                  │ (AI's Soul)     │
                  └────────┬────────┘
                           │
                  ┌────────▼────────┐
                  │ 08 Voice+Buddy  │
                  │ (Special)       │
                  └─────────────────┘
```

### Chapter Details

> **Note:** The course content is currently available in Chinese only. English translations are coming soon in the `en/` directory.

| # | File | Topic | Core Source Files | Est. Time |
|---|------|-------|-------------------|-----------|
| 00 | [00_index.md](zh-CN/00_index.md) | Index & Architecture Overview | — | 10min |
| 01 | [01_architecture_overview.md](zh-CN/01_architecture_overview.md) | Global Architecture | `main.tsx`, `App.tsx`, `QueryEngine.ts` | 30min |
| 02 | [02_tool_system.md](zh-CN/02_tool_system.md) | Tool System Deep Dive | `Tool.ts`, `tools.ts`, `GlobTool.ts` | 45min |
| 03 | [03_permission_security.md](zh-CN/03_permission_security.md) | Permission & Security | `permissions.ts`, `filesystem.ts` | 45min |
| 04 | [04_query_loop_api.md](zh-CN/04_query_loop_api.md) | Query Loop & API (Advanced) | `query.ts`, `StreamingToolExecutor.ts` | 50min |
| 05 | [05_multi_agent_system.md](zh-CN/05_multi_agent_system.md) | Multi-Agent + Coordinator (Advanced) | `AgentTool.tsx`, `runAgent.ts`, `coordinatorMode.ts` | 50min |
| 06 | [06_mcp_extensions.md](zh-CN/06_mcp_extensions.md) | MCP, Skills & Extensions | `mcp/client.ts`, `skills/`, `bundledSkills.ts` | 45min |
| 07 | [07_prompt_engineering.md](zh-CN/07_prompt_engineering.md) | Prompt Engineering Deep Dive | `prompts.ts`, `BashTool/prompt.ts`, `compact/prompt.ts` | 60min |
| 08 | [08_voice_buddy.md](zh-CN/08_voice_buddy.md) | Voice Mode & Buddy System | `voiceStreamSTT.ts`, `useVoice.ts`, `buddy/` | 30min |

**Total ~5.5 hours** for a systematic understanding of Claude Code's core architecture.

## Suggested Learning Paths

### Quick Start (2 hours)
```
00 → 01 → 02 → 04
```
Understand: Entry point → Tool definitions → Query loop

### Security Research (1.5 hours)
```
00 → 03 → 02 (permission section)
```
Understand: Permission model → Command security analysis → Sandbox

### Agent Research (2.5 hours)
```
00 → 05 → 04 → 06
```
Understand: Agent architecture + Coordinator → Query loop → MCP/Skills extensions

### Prompt Research (1 hour)
```
00 → 07
```
Understand: System prompts → Tool prompts → Agent prompts → Compression prompts → Cache optimization

### Special Features (30 minutes)
```
08
```
Understand: Voice hold-to-talk → WebSocket STT → Buddy deterministic generation → ASCII animation

### Complete Course (5.5 hours)
```
00 → 01 → 02 → 03 → 04 → 05 → 06 → 07 → 08
```

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                   Claude Code Architecture                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌──────────┐    ┌──────────────┐    ┌──────────────────┐  │
│  │ CLI/REPL │    │ Bridge/IDE   │    │ SDK/Headless     │  │
│  │ (Ink UI) │    │ (VS Code)    │    │ (Programmatic)   │  │
│  └────┬─────┘    └──────┬───────┘    └────────┬─────────┘  │
│       │                 │                      │            │
│       └─────────────────┼──────────────────────┘            │
│                         │                                   │
│                  ┌──────▼──────┐                            │
│                  │  App State  │                             │
│                  └──────┬──────┘                            │
│                         │                                   │
│                  ┌──────▼──────┐                            │
│                  │ QueryEngine │  ← System Heart            │
│                  │  query()    │                            │
│                  └──┬───┬──┬──┘                            │
│                     │   │  │                                │
│          ┌──────────┘   │  └──────────┐                    │
│          │              │              │                    │
│   ┌──────▼──────┐ ┌────▼─────┐ ┌──────▼──────┐            │
│   │ Tool System │ │ Claude   │ │ Permission  │            │
│   │ 40+ Tools   │ │ API      │ │ System      │            │
│   └──────┬──────┘ └──────────┘ │ 5-Layer     │            │
│          │                      │ Defense     │            │
│          │                      └─────────────┘            │
│   ┌──────┴──────────────────────────────┐                  │
│   │  Built-in Tools    MCP Tools        │                  │
│   │  ├── Bash          ├── mcp__db__*   │                  │
│   │  ├── Read/Edit     ├── mcp__api__*  │                  │
│   │  ├── Agent         └── ...          │                  │
│   │  ├── WebSearch                      │                  │
│   │  └── 30+ more                       │                  │
│   └─────────────────────────────────────┘                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Key Source File Reference

| File | Size | Function |
|------|------|----------|
| `src/main.tsx` | 785KB | CLI entry, Commander.js command definitions |
| `src/Tool.ts` | 28KB | **Tool interface** — the "constitution" for all tools |
| `src/tools.ts` | 16KB | **Tool registry** — determines available tools |
| `src/query.ts` | 67KB | **Core query loop** — the system's heart |
| `src/QueryEngine.ts` | 45KB | Query engine wrapper |
| `src/utils/permissions/permissions.ts` | 51KB | **Permission check core** |
| `src/utils/permissions/filesystem.ts` | 61KB | Filesystem permissions |
| `src/tools/BashTool/bashSecurity.ts` | 100KB | Command security analysis |
| `src/tools/AgentTool/AgentTool.tsx` | ~50KB | Agent tool implementation |
| `src/tools/AgentTool/runAgent.ts` | 34KB | Agent lifecycle |
| `src/services/mcp/client.ts` | 116KB | MCP client |
| `src/services/tools/StreamingToolExecutor.ts` | 17KB | Streaming tool executor |
| `src/services/tools/toolOrchestration.ts` | 5KB | Tool orchestration (concurrent/serial) |

## Core Design Principles

### 1. Fail-Closed Security First
```typescript
// All tools default to unsafe assumptions
const TOOL_DEFAULTS = {
  isConcurrencySafe: () => false,  // If unsure, don't parallelize
  isReadOnly: () => false,          // If unsure, assume it writes
};
```

### 2. Defense in Depth
```
Layer 1: Permission Rules (deny > ask > allow)
  └── Layer 2: Permission Mode (default/plan/bypass/auto)
      └── Layer 3: Tool-specific checks (BashTool security)
          └── Layer 4: Path safety checks (filesystem)
              └── Layer 5: Sandbox (macOS Seatbelt)
```

### 3. Streaming Tool Execution
```
Claude API streams tool_use blocks
  → Execution begins as soon as complete input is received
  → Concurrency-safe tools run in parallel
  → Results returned in order
```

### 4. Prompt Cache Optimization
```
Tool list sorted → Stable ordering → Maximize cache hit rate
Built-in tools first → MCP tools after → Prefix unchanged
```

## Key Discoveries Per Chapter

| Chapter | Most Valuable Discovery |
|---------|------------------------|
| 01 Architecture | React/Ink terminal UI + functional state management — CLI can use React too |
| 02 Tools | `buildTool()` factory pattern + Zod schema as prompt, 18+ Feature Flags |
| 03 Security | 5-layer defense in depth; bypass mode still has safety floor (.git/.claude cannot be bypassed) |
| 04 Query | `StreamingToolExecutor` streaming concurrent execution, 3-layer AbortController cascade, 5-layer compression pipeline |
| 05 Agent | Default isolation with explicit sharing, fork prompt cache sharing, Coordinator orchestrator pattern, 10-step finally cleanup |
| 06 MCP/Skills | Skills 3-layer architecture, bundled skill lazy extraction + 6-layer security, /simplify 3-agent parallel review |
| 07 Prompt | 7 categories with 40+ prompt files, static/dynamic separation for cache optimization, Meta-Prompt generates Agents |
| 08 Voice/Buddy | Record-first connect-later eliminates latency, Buddy deterministic generation + Bones not persisted to prevent forgery |

## Transferable Design Patterns

One of the most valuable aspects of this analysis is the **11 design patterns** extracted from Claude Code that can be applied to any Agent system:

### From the Query Loop (Ch.04)

| Pattern | In Claude Code | Universal Application |
|---------|---------------|----------------------|
| **Optimistic Recovery** | Withhold error messages, attempt recovery, only expose on failure | API gateways, DB connection pools, leader election |
| **Layered Degradation** | 5-layer compression pipeline — cheap first, expensive last | CDN caching (memory → disk → origin), search degradation |
| **State Machine + Transition Log** | `while(true)` + `transition` field prevents recovery loops | Workflow engines, game AI, compiler optimization passes |
| **Read-Write Lock Concurrency** | `ConcurrencySafe` = read lock, non-safe = write lock | Database MVCC, filesystem flock, K8s admission webhooks |
| **Immutable Config Snapshot** | `buildQueryConfig()` snapshots once at entry | React/Redux stores, game frame updates, microservice config |
| **Hierarchical Cancellation** | 3-layer AbortController with semantic abort reasons | Microservice request cancellation, CI/CD pipelines, browser fetch |

### From the Multi-Agent System (Ch.05)

| Pattern | In Claude Code | Universal Application |
|---------|---------------|----------------------|
| **Capability-based Security** | `createSubagentContext` — default isolated, explicit opt-in | Browser iframe sandbox, Docker capabilities, API gateways |
| **Cache-Friendly Forking** | Fork child agents with exact cache key parameters | Linux fork() + CoW, CDN layered caching, Git packfiles |
| **Deterministic Cleanup** | 12-step `finally` block, reverse acquisition order | DB connection pools, file handles, K8s Pod termination |
| **Star Topology Orchestration** | Coordinator + Workers, no direct worker-to-worker communication | MapReduce, Saga pattern, CI/CD pipelines |
| **Monotonic Permission Narrowing** | Child agents can narrow but never widen parent permissions | Unix setuid, OAuth scope inheritance, AWS IAM boundaries |

## Design Decision Deep Dives

The notes don't just describe *what* the code does — they explain *why* certain approaches were chosen over alternatives:

### Why Read-Write Locks Over Actor Model or DAG? (Ch.04)

```
┌─────────────────────────────────────────────────────────────────┐
│ Three Paradigms for Agent Tool Orchestration                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ Option A: Actor Model (Erlang/Elixir style)                     │
│   ✗ JS is single-threaded → Actors degrade to Promises          │
│   ✗ Sibling abort needs supervisor trees → over-engineering      │
│                                                                 │
│ Option B: DAG Scheduling (LangGraph style)                      │
│   ✗ LLM decides tools at runtime → can't pre-build DAG          │
│   ✗ Must wait for complete response → loses streaming advantage  │
│                                                                 │
│ Option C: Read-Write Lock + Streaming Add (Claude Code) ✓       │
│   ✓ Simple intuition (reads parallel, writes exclusive)          │
│   ✓ Supports streaming add (no need to wait for full response)   │
│   ✓ Sibling abort naturally fits (shared AbortController)        │
│                                                                 │
│ Key Insight: LLMs typically output tool calls in logical order   │
│ (Read before Edit), so simple read-write locks suffice.          │
└─────────────────────────────────────────────────────────────────┘
```

### Worker Communication: Three Mechanisms Compared (Ch.05)

| Mechanism | Direction | Timing | Use Case |
|-----------|-----------|--------|----------|
| **task-notification** | Worker → Coordinator | Async queue, consumed on next iteration | Completion/failure signals |
| **SendMessage** | Coordinator → Worker | Via filesystem mailbox | Continue existing worker's task |
| **Scratchpad files** | Worker ↔ Worker | No ordering guarantee | Shared research results, intermediate data |

**Rule of thumb**: Control flow uses task-notification + SendMessage; data flow uses Scratchpad.

## Project Structure

```
learning-notebooks/
├── README.md              ← English README (you are here)
├── README_CN.md           ← Chinese README
├── LICENSE
├── .gitignore
├── zh-CN/                 ← Chinese course content
│   ├── 00_index.md        ← Index & architecture overview
│   ├── 01_architecture_overview.md
│   ├── 02_tool_system.md
│   ├── 03_permission_security.md
│   ├── 04_query_loop_api.md
│   ├── 05_multi_agent_system.md
│   ├── 06_mcp_extensions.md
│   ├── 07_prompt_engineering.md
│   └── 08_voice_buddy.md
└── en/                    ← English translations (coming soon)
```

## How to Use

### Option 1: Browse on GitHub
GitHub natively renders Markdown — just click any `.md` file to read.

### Option 2: VS Code
Open `.md` files in VS Code and use Markdown Preview.

### Option 3: Any Markdown Reader
All chapters are standard Markdown format, compatible with any Markdown viewer.

## FAQ

**Q: Do I need access to Claude Code's source code to follow along?**
A: No. The notes are self-contained with all relevant code snippets and architecture diagrams included. However, having the source code open alongside can deepen your understanding.

**Q: Is this based on a specific version of Claude Code?**
A: The analysis is based on the source code extracted from the npm package's source map files. The core architecture patterns are stable across versions, though specific implementation details may change.

**Q: Can I apply these patterns to non-TypeScript projects?**
A: Absolutely. The 11 transferable design patterns (Ch.04 & Ch.05) are language-agnostic. The read-write lock concurrency model, hierarchical cancellation, and star topology orchestration work in any language.

**Q: What's the difference between the Chinese and English versions?**
A: The course content (in `zh-CN/`) is currently in Chinese only. This English README provides a comprehensive overview, architecture diagrams, design pattern summaries, and navigation to the Chinese chapters. Full English translations are planned for the `en/` directory.

**Q: How does this compare to official Claude Code documentation?**
A: The official docs focus on *how to use* Claude Code. This project focuses on *how it's built* — the internal architecture, design decisions, and engineering patterns that make it work.

## Disclaimer

This project is for **educational and research purposes only**. The source code was obtained from publicly exposed Claude Code TypeScript source via npm package source map files. This project does not contain any original source files — only architectural and design pattern analysis notes.

## License

These learning notes are released under the MIT License. Note that the analyzed Claude Code source code is copyrighted by Anthropic.

## Contributing

Issues and PRs are welcome! If you discover interesting design patterns or security mechanisms, feel free to add them to the notes.

**How to contribute:**
1. Fork this repository
2. Create your branch (`git checkout -b feature/amazing-discovery`)
3. Commit your changes (`git commit -m 'Add: discovered interesting design in XXX'`)
4. Push to your branch (`git push origin feature/amazing-discovery`)
5. Submit a Pull Request

## Sister Project: nano-claude-code

[![GitHub](https://img.shields.io/badge/GitHub-nano--claude--code-green?logo=github)](https://github.com/dadiaomengmeimei/nano-claude-code)

> **1,646 lines of TypeScript · 15 source files · 4 runtime dependencies.**
> A minimal, hackable AI coding assistant for the terminal — inspired by Claude Code, distilled to its essence.

If this project (Learning Notebook) is a **deep dive** into Claude Code's 512K+ lines of source code, then [nano-claude-code](https://github.com/dadiaomengmeimei/nano-claude-code) is its core architecture **rewritten from scratch** as a minimal implementation:

| Metric | Claude Code | nano-claude-code |
|--------|-------------|------------------|
| Source files | ~1,900 | **15** |
| Lines of code | 512,000+ | **1,646** |
| Runtime dependencies | 50+ | **4** |
| Tools | 40+ | **6** |
| Runtime | Bun | **Node.js ≥ 20** |

**Core Features:**
- 🔄 **Agent Loop** — Query → tool calls → execute → feed results back → reasoning (up to 30 rounds)
- 🌊 **Streaming Output** — Real-time token-by-token display as the LLM thinks
- 🛠️ **6 Essential Tools** — Bash, FileRead, FileEdit, FileWrite, Grep, Glob
- 🔐 **Permission System** — Read-only tools auto-allowed; write/shell operations require confirmation
- 📋 **Context Awareness** — Auto-reads `CLAUDE.md`, detects project type, gathers Git info
- 🔌 **Provider Abstraction** — `LLMProvider` interface ready for OpenAI/Ollama/local models

**Recommended workflow:** First study the architecture through the Learning Notebook, then get hands-on with nano-claude-code to build your own AI coding assistant from scratch.

👉 **[GitHub: nano-claude-code](https://github.com/dadiaomengmeimei/nano-claude-code)**

## Links

- [Claude Code Official Docs](https://docs.anthropic.com/en/docs/claude-code)
- [Anthropic Website](https://www.anthropic.com/)
- [MCP Protocol Specification](https://modelcontextprotocol.io/)
- [This Project on GitHub](https://github.com/dadiaomengmeimei/claude-code-sourcemap-learning-notebook)
- [nano-claude-code](https://github.com/dadiaomengmeimei/nano-claude-code) — Minimal Claude Code implementation

---

**If this project helps you, please give it a Star! ⭐**

**Follow [@dadiaomengmeimei](https://github.com/dadiaomengmeimei) for more AI tool source code analysis!**
