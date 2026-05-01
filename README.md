# GenX Multi-Agent System

> Unified architecture for multi-expert AI agent collaboration

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                     Orchestrator                            │
│   (Manager · Lifecycle · Heartbeat ·跨机器协调)             │
└──────────────────────┬──────────────────────────────────────┘
                       │
              ┌────────▼────────┐
              │   Event Bus     │
              │     (Bell)      │
              │ Pub/Sub · 消息协议 │
              └────────┬────────┘
                       │
     ┌─────────────────┼─────────────────┐
     │                 │                 │
┌────▼────┐      ┌────▼────┐      ┌────▼────┐
│  Gen    │      │   ML    │      │ Custom  │
│  Code   │      │Researcher│     │ Agents  │
└─────────┘      └─────────┘      └─────────┘
```

## Core Components

| Component | Repo | Responsibility |
|-----------|------|----------------|
| **Orchestrator** | [orchestrator](https://github.com/genai-io/orchestrator) | Agent lifecycle, coordination, distributed scheduling |
| **Event Bus** | [bell](https://github.com/genai-io/bell) | Pub/Sub messaging, inter-agent communication |
| **Gen Code** | [gen-code](https://github.com/genai-io/gen-code) | General-purpose coding agent |
| **ML Researcher** | [ml-researcher](https://github.com/genai-io/ml-researcher) | ML research agent (experiments, training, evaluation) |

## Project Levels

```
Project
 ├── Skill         # 单个能力定义
 ├── SubAgent      # 子代理配置
 ├── MCP           # Model Context Protocol 工具
 └── Agent          # 可执行实体
```

## Design Principles

1. **Event-driven** — All agents communicate via Bell event bus
2. **Distributed** — Orchestrator can manage agents across machines
3. **Hierarchical** — Manager controls agent lifecycle, agents are workers
4. **Extensible** — New agents plug into the bus via standard protocol

## Communication Flow

```
User/CLI
    │
    ▼
Orchestrator ──creates/destroys──► Agent Pool
    │
    │ publishes events
    ▼
Bell (Event Bus) ◄───subscribes──── Agent (GenCode / ML-Researcher / ...)
    │
    │ emits results
    ▼
Orchestrator (routes to next agent or returns to user)
```

## License

Apache 2.0