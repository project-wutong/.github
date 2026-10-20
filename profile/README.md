# Project Wutong · 梧桐计划

**本地优先、可验证、开放的知识与协作套件。** Wutong Protocol（WTP `wutong/0.1`，规范修订 v0.1.2）
定义了签名对象、文档、Agent、同步与联邦的互通格式；Wutong Suit（梧桐套件）在其之上提供桌面应用与可选的网络组件。

*A local-first, verifiable, open suite for knowledge and collaboration, built on the Wutong Protocol.*

- 🦀 **Rust 优先** — 协议只在 Rust 中实现一次；桌面应用仅界面层使用 C++/Qt 6，经 `cxx` 调用 Rust 引擎。
- 🏠 **本地优先** — 无需账户、网络、服务器或 AI 即可使用；网络能力完全可选。
- ✅ **可验证** — 跨语言测试向量保证 Rust / Go / Python / TypeScript / C++ / C# 结果逐字节一致。
- ⚖️ **AGPL-3.0-only**

| 仓库 Repository | 说明 Description | 语言 | CI |
|---|---|---|---|
| [`wutong-protocol`](https://github.com/project-wutong/wutong-protocol) | 规范、Schema、示例、跨语言测试向量、参考实现 · Spec, schemas, vectors | Markdown/JSON | [![CI](https://github.com/project-wutong/wutong-protocol/actions/workflows/ci.yml/badge.svg)](https://github.com/project-wutong/wutong-protocol/actions/workflows/ci.yml) |
| [`wutong-core`](https://github.com/project-wutong/wutong-core) | Rust 核心（Rust SDK）+ C ABI · Rust core and C ABI | Rust | [![CI](https://github.com/project-wutong/wutong-core/actions/workflows/ci.yml/badge.svg)](https://github.com/project-wutong/wutong-core/actions/workflows/ci.yml) |
| [`wutong-cli`](https://github.com/project-wutong/wutong-cli) | `wutong` 命令行 · CLI | Rust | [![CI](https://github.com/project-wutong/wutong-cli/actions/workflows/ci.yml/badge.svg)](https://github.com/project-wutong/wutong-cli/actions/workflows/ci.yml) |
| [`wutong-networkd`](https://github.com/project-wutong/wutong-networkd) | 可选本机网络守护进程 · Opt-in network daemon | Rust | [![CI](https://github.com/project-wutong/wutong-networkd/actions/workflows/ci.yml/badge.svg)](https://github.com/project-wutong/wutong-networkd/actions/workflows/ci.yml) |
| [`wutong-server`](https://github.com/project-wutong/wutong-server) | 可选自托管节点 · Optional self-hosted node | Rust | [![CI](https://github.com/project-wutong/wutong-server/actions/workflows/ci.yml/badge.svg)](https://github.com/project-wutong/wutong-server/actions/workflows/ci.yml) |
| [`wutong-writer`](https://github.com/project-wutong/wutong-writer) | 梧桐写作 · Writer | Rust + Qt 6 | [![CI](https://github.com/project-wutong/wutong-writer/actions/workflows/ci.yml/badge.svg)](https://github.com/project-wutong/wutong-writer/actions/workflows/ci.yml) |
| [`wutong-knowledge`](https://github.com/project-wutong/wutong-knowledge) | 梧桐知识库 · Knowledge | Rust + Qt 6 | [![CI](https://github.com/project-wutong/wutong-knowledge/actions/workflows/ci.yml/badge.svg)](https://github.com/project-wutong/wutong-knowledge/actions/workflows/ci.yml) |
| [`wutong-publisher`](https://github.com/project-wutong/wutong-publisher) | 梧桐发布 · Publisher | Rust + Qt 6 | [![CI](https://github.com/project-wutong/wutong-publisher/actions/workflows/ci.yml/badge.svg)](https://github.com/project-wutong/wutong-publisher/actions/workflows/ci.yml) |
| [`wutong-network`](https://github.com/project-wutong/wutong-network) | 梧桐网络 · Network GUI | Rust + Qt 6 | [![CI](https://github.com/project-wutong/wutong-network/actions/workflows/ci.yml/badge.svg)](https://github.com/project-wutong/wutong-network/actions/workflows/ci.yml) |
| [`wutong-builder`](https://github.com/project-wutong/wutong-builder) | 梧桐构建 · Builder | Rust + Qt 6 | [![CI](https://github.com/project-wutong/wutong-builder/actions/workflows/ci.yml/badge.svg)](https://github.com/project-wutong/wutong-builder/actions/workflows/ci.yml) |
| [`wutong-sdk-cpp`](https://github.com/project-wutong/wutong-sdk-cpp) | C++ SDK | C++20 | [![CI](https://github.com/project-wutong/wutong-sdk-cpp/actions/workflows/ci.yml/badge.svg)](https://github.com/project-wutong/wutong-sdk-cpp/actions/workflows/ci.yml) |
| [`wutong-sdk-python`](https://github.com/project-wutong/wutong-sdk-python) | Python SDK | Rust/PyO3 | [![CI](https://github.com/project-wutong/wutong-sdk-python/actions/workflows/ci.yml/badge.svg)](https://github.com/project-wutong/wutong-sdk-python/actions/workflows/ci.yml) |
| [`wutong-sdk-typescript`](https://github.com/project-wutong/wutong-sdk-typescript) | TypeScript SDK | Rust/wasm + TS | [![CI](https://github.com/project-wutong/wutong-sdk-typescript/actions/workflows/ci.yml/badge.svg)](https://github.com/project-wutong/wutong-sdk-typescript/actions/workflows/ci.yml) |
| [`wutong-sdk-go`](https://github.com/project-wutong/wutong-sdk-go) | Go SDK | Go | [![CI](https://github.com/project-wutong/wutong-sdk-go/actions/workflows/ci.yml/badge.svg)](https://github.com/project-wutong/wutong-sdk-go/actions/workflows/ci.yml) |
| [`wutong-sdk-csharp`](https://github.com/project-wutong/wutong-sdk-csharp) | .NET SDK | C# | [![CI](https://github.com/project-wutong/wutong-sdk-csharp/actions/workflows/ci.yml/badge.svg)](https://github.com/project-wutong/wutong-sdk-csharp/actions/workflows/ci.yml) |

📋 路线图 Roadmap: [Wutong Roadmap](https://github.com/orgs/project-wutong/projects) · 计划 Plan: [PLAN.md](https://github.com/project-wutong/.github/blob/main/PLAN.md)
· 贡献 Contributing: [CONTRIBUTING.md](https://github.com/project-wutong/.github/blob/main/CONTRIBUTING.md)
· 安全 Security: [SECURITY.md](https://github.com/project-wutong/.github/blob/main/SECURITY.md)

> Security, Sync and Federation profiles are experimental and not yet independently audited.
