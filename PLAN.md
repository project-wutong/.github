# Project Wutong（梧桐计划）工程与项目管理计划

> 版本：2026-10-20 · 维护者：Wutong Dev <wutong-dev@mail.grokbot.com> · GitHub 组织：[`project-wutong`](https://github.com/project-wutong)
>
> 依据文档：《Project Wutong 架构 v1.4》《WTP-STD-001 梧桐协议 v0.1.2 正式标准规范》《v0.1.2 发布演进与多语言工程实现指南》《v0.1.2 一致性与审计报告》，以及历史草案 WTP-000 v0.1.0-rc.1（含 §18.2 发行阻断项与 §21 后续工作排序）。

## 1. 总体原则

| 原则 | 说明 |
|---|---|
| Rust 优先 | 凡能用 Rust 的部分一律用 Rust（Edition 2024，MSRV 1.85）。协议规则只在 Rust 中实现一次（`wutong-core`）。 |
| GUI = C++/Qt，仅限界面层 | 桌面应用的界面层使用 C++23 + Qt 6（Widgets）。文件格式、协议、索引、校验等全部在 Rust 引擎 crate 中，经 [`cxx`](https://cxx.rs) 桥接给 C++。 |
| 本地优先 | 所有应用无需账户、网络、Server 或 AI 即可工作；网络能力（`wutong-networkd` / `wutong-server`）均为可选。 |
| 一库一仓 | 协议与每个套件组件、每种语言 SDK 各自独立仓库，`main` 受保护，只经 PR + CI 合入。 |
| 可验证 | 跨语言向量（`wutong-protocol/vectors/crosslang`）是所有实现的共同真值；各仓 CI 检查向量漂移。 |
| 许可 | 全部自有代码 `AGPL-3.0-only`，每个源文件 SPDX 头，REUSE 3.3；依赖许可证由 cargo-deny 在 CI 强制。 |

## 2. 仓库清单

| 仓库 | 用途 | 语言 | 主要依赖 |
|---|---|---|---|
| [`.github`](https://github.com/project-wutong/.github) | 组织主页（profile/README）、默认社区健康文件、Issue/PR 模板、本计划 | Markdown | — |
| [`wutong-protocol`](https://github.com/project-wutong/wutong-protocol) | 规范正文（WTP-STD-001 v0.1.2）、JSON Schema、OpenAPI、示例、测试向量、WIP 提案流程；保留 Python/Node/C++23 参考实现作为**独立一致性实现** | Markdown / JSON；参考实现 Python、JS、C++23 | Python `cryptography`、`jsonschema`；Node；CMake |
| [`wutong-core`](https://github.com/project-wutong/wutong-core) | **Rust 核心**（即 Rust SDK）：`wtp-types`（Schema 生成的 24 类消息类型）、`wtp-wire`（严格 JSON、JCS、dCBOR、Schema 校验）、`wtp-crypto`（Ed25519 签名/验签、重放账本）、`wtp-agent`（Task 状态机）、`wtp-document`（路径、补丁、PROV）、`wtp-policy`（能力授权）、`wutong-core` facade、`wutong-ffi`（稳定 C ABI） | Rust | ed25519-dalek 2、jsonschema 0.59、serde_json、sha2、base64 |
| [`wutong-cli`](https://github.com/project-wutong/wutong-cli) | `wutong` 命令行：digest / validate / canon / verify / info | Rust | wutong-core、clap 4 |
| [`wutong-networkd`](https://github.com/project-wutong/wutong-networkd) | 可选的本机网络守护进程（opt-in 配置、JSON-lines IPC、Unix socket 0600），后续承载联邦与同步 | Rust | wutong-core、toml、serde |
| [`wutong-server`](https://github.com/project-wutong/wutong-server) | 可选自托管无头节点：联邦 HTTPS 绑定（`/_wutong/v0.1/meta`、`/objects`）、同步中继、服务宿主、Agent worker | Rust | wutong-core、axum 0.8、tokio |
| [`wutong-writer`](https://github.com/project-wutong/wutong-writer) | 梧桐写作：源码优先的 Markdown/文本编辑器（换行/BOM 保真、无改动不写盘、外部修改检测） | Rust 引擎 + C++/Qt 6 GUI | cxx、Qt6 Widgets |
| [`wutong-knowledge`](https://github.com/project-wutong/wutong-knowledge) | 梧桐知识库：本地 Vault 索引、检索、关系 | Rust 引擎 + C++/Qt 6 GUI | cxx、Qt6 |
| [`wutong-publisher`](https://github.com/project-wutong/wutong-publisher) | 梧桐发布：格式识别与导出工具链（Typst/Pandoc 等）调度 | Rust 引擎 + C++/Qt 6 GUI | cxx、Qt6 |
| [`wutong-network`](https://github.com/project-wutong/wutong-network) | 梧桐网络：`wutong-networkd` 的图形前端 | Rust 引擎 + C++/Qt 6 GUI | cxx、Qt6 |
| [`wutong-builder`](https://github.com/project-wutong/wutong-builder) | 梧桐构建：服务规格编辑、`spec_digest`、密钥泄露扫描 | Rust 引擎 + C++/Qt 6 GUI | cxx、Qt6 |
| [`wutong-sdk-cpp`](https://github.com/project-wutong/wutong-sdk-cpp) | C++20 头文件 SDK（RAII 包装 `wutong-ffi` C ABI） | C++ / CMake | wutong-ffi |
| [`wutong-sdk-python`](https://github.com/project-wutong/wutong-sdk-python) | Python SDK（PyO3 + maturin，abi3 wheel） | Rust + Python | wutong-core、pyo3 0.30 |
| [`wutong-sdk-typescript`](https://github.com/project-wutong/wutong-sdk-typescript) | TypeScript SDK（Rust → WebAssembly + TS 类型包装） | Rust + TypeScript | wutong-core、wasm-bindgen |
| [`wutong-sdk-go`](https://github.com/project-wutong/wutong-sdk-go) | Go SDK：**原生纯 Go**（文档要求避免强制 cgo），以跨语言向量锁定一致性 | Go | 标准库 |
| [`wutong-sdk-csharp`](https://github.com/project-wutong/wutong-sdk-csharp) | .NET SDK（`LibraryImport` P/Invoke 调用 `wutong-ffi`） | C# | wutong-ffi |

> Rust SDK 即 `wutong-core` 本身（crates `wutong-core` + 子 crate），不另建仓库。
> 文档中的“六语言 SDK”= Rust、C++、Python、TypeScript、Go、C#。

### 依赖关系

```
wutong-protocol (规范/Schema/向量)  ──向量同步(CI)──▶  wutong-core ──git dep──▶ cli / networkd / server / 5×GUI 引擎 / sdk-python / sdk-typescript
                                                          └─ wutong-ffi (C ABI) ──▶ sdk-cpp / sdk-csharp
wutong-protocol/vectors ──复制+CI 漂移检查──▶ sdk-go (纯 Go)
```

下游仓库通过 `git = "https://github.com/project-wutong/wutong-core", branch = "main"` 依赖核心，并提交 `Cargo.lock`、CI 使用 `--locked`（可复现）。核心发布 crates.io 后改为版本依赖（见 Issue）。

## 3. 工程规范（所有仓库一致）

- 默认分支 `main`；启用规则集：禁止直接推送与强推，必须 PR，必须 CI 通过（必需检查按仓库配置）。
- Conventional Commits；每个提交 `git commit -s`（DCO 1.1）；作者 `Wutong Dev <wutong-dev@mail.grokbot.com>`。
- CI：Rust 仓库 `cargo fmt --check`、`clippy -D warnings`、`cargo test --locked`（三平台矩阵按需）、`cargo-deny`（许可证/安全公告/来源）；GUI 仓库额外 CMake + Qt 构建与 offscreen 冒烟测试；SDK 仓库各自语言的测试与格式检查。
- Dependabot 每周（cargo / gomod / pip / npm / nuget / github-actions）。
- 社区文件：README（中英）、LICENSE、CONTRIBUTING、SECURITY、CODE_OF_CONDUCT、CHANGELOG（Keep a Changelog）、Issue/PR 模板、CODEOWNERS、REUSE.toml。
- 协议变更走 WIP（Wutong Improvement Proposal）：在 `wutong-protocol/wip/` 提交，标签 `type: spec-change`。

### 统一标签

| 组 | 标签 |
|---|---|
| 类型 | `type: bug` `type: feature` `type: docs` `type: chore` `type: security` `type: spec-change` `type: release-blocker` |
| 优先级 | `priority: P0` `priority: P1` `priority: P2` |
| Profile | `profile: core` `profile: document` `profile: agent` `profile: security` `profile: sync` `profile: federation` `profile: service` `profile: interop` |
| 状态 | `status: triage` `status: blocked` `status: needs-decision` |
| 社区 | `good first issue` `help wanted` |

## 4. 里程碑

| 里程碑 | 目标 | 退出标准 |
|---|---|---|
| **v0.1.2** — Core 正式版 | 关闭 rc.1 §18.2 全部 6 项 Core 阻断项；Rust 核心、CLI、GUI 骨架与 SDK 通过共同向量 | 所有 `type: release-blocker` 关闭；三平台 CI 全绿；一致性测试包与迁移说明发布 |
| **v0.2.0** — 协作与联邦 | CRDT 评测与 Sync Profile、原生 Federation 双节点、Agent 任务执行器、Builder/Server 最小服务、六语言 SDK 共同 fixtures | 双节点互通测试、CRDT 基准报告、SDK 端到端样例 |
| **v0.3.0** — 生产安全 | Security/Sync/Federation 生产支持：信任根/轮换/撤销、E2EE 标识、模糊测试、故障注入、SSRF/ACL/恶意节点、第三方安全评审、外部协议适配器 | 第三方审计报告；模糊测试持续运行 |

## 5. Issue 拆分

### 5.1 rc.1 §18.2 Core 发行阻断项 → `type: release-blocker`，里程碑 v0.1.2

| # | 阻断项 | 仓库 | 当前状态 |
|---|---|---|---|
| RB-1 | 移除示例 `$id`，确定域名与正式 Schema 发布地址；冻结 Registry 与必需字段 | wutong-protocol | 需用户决策域名（`status: needs-decision`） |
| RB-2 | Rust 2024 独立解析/验证器，通过与 C++23/Python/TypeScript 的 Wire 向量 | wutong-core | **已实现**：24 类示例 JCS/dCBOR/签名与 Python 逐字节一致；待补 Node 侧交叉验证进 CI 后关闭 |
| RB-3 | 至少两个独立实现运行字符串编码/数字/签名/坏输入/深度尺寸/重复键边界测试 | wutong-protocol | 部分：已有 `crosslang/` 向量（Rust、Go、Python 参考实现），需扩充边界向量 |
| RB-4 | Object 身份复制语义、Revision DAG 校验、未知扩展保留与错误映射 | wutong-protocol（规范）+ wutong-core（实现） | 未开始 |
| RB-5 | 不依赖 Network/Server 的 Win/macOS/Linux 本地应用测试，含文件无损 round-trip 与外部编辑器并发修改 | wutong-writer（+ 其他 GUI） | 部分：Writer 引擎已有 BOM/换行保真与外部修改检测单测 |
| RB-6 | 发布一致性测试包、分版本迁移说明、变更日志与安全报告 | wutong-protocol | 部分：CHANGELOG 已建立 |

### 5.2 rc.1 §21 后续工作排序

| 优先级 | 工作项 | 仓库 | 里程碑 |
|---|---|---|---|
| P0 | Rust 2024 独立 Core + Wire parser（同 RB-2） | wutong-core | v0.1.2 |
| P0 | FileCoordinator 三平台崩溃恢复（prepared/replaced/committed、symlink、外部锁、权限/元数据） | wutong-core（新 crate `wtp-fs`） | v0.1.2 |
| P0 | Key Binding / Rotation / Revocation（KeyRegistry 泄露回溯防御、委托时序单调） | wutong-core | v0.1.2 |
| P0 | Schema 与 Registry 正式托管（同 RB-1） | wutong-protocol | v0.1.2 |
| P1 | CRDT 实际评测（Yrs vs Automerge） | wutong-core | v0.2.0 |
| P1 | 原生 Federation 双节点（Resolve/Fetch/Subscribe） | wutong-server + wutong-networkd | v0.2.0 |
| P1 | Agent 审阅/任务执行器 | wutong-server | v0.2.0 |
| P1 | Builder/Server 最小服务（静态文档站/博客发布与权限隔离） | wutong-builder + wutong-server | v0.2.0 |
| P2 | 六语言 SDK 共同 fixtures、升级/降级、端到端样例 | 各 SDK 仓库 | v0.2.0 |
| P2 | 外部协议适配器（AT Protocol、ActivityPub、WordPress、Matrix） | wutong-publisher | v0.3.0 |

### 5.3 各组件后续 Issue（摘要，完整列表见 `ISSUES.json`）

- **wutong-core**：C ABI 增加 sign/verify；cargo-fuzz 模糊测试；持久化重放账本；能力链完整验证；crates.io 发布。
- **wutong-cli**：`sign`、补丁应用、索引构建、对 networkd 的可选调用；cargo-dist 发布二进制。
- **wutong-networkd / wutong-server**：签名回执闭环、RFC 9421 HTTP 消息签名、Windows 命名管道 IPC、SSRF/ACL 测试。
- **GUI 仓库**：真实编辑/索引/导出界面；Qt 版本基线决策；Qt 许可合规（LGPL 动态链接）。
- **SDK 仓库**：发布渠道（PyPI / npm / NuGet / Go module tag / CMake install+vcpkg）；Go SDK 补 Schema 校验与能力链。
- **文档歧义**：每条一个 `status: needs-decision` Issue，位于 `wutong-protocol`（见 §7）。

## 6. 文档与 “Rust 优先” 政策的冲突及处理

| 文档原文/隐含要求 | 冲突 | 处理 |
|---|---|---|
| 实现指南/报告：C++23 原生 Core（`reference/src/core.cpp`）、C++ + Qt 应用 | 用户要求 Rust 优先，GUI 仅界面层 C++ | **Rust 核心为唯一正式实现**；C++23 参考核心原样保留在 `wutong-protocol/reference` 作为**独立一致性实现**（满足 RB-3 “两个独立实现”）；Qt 应用只含界面，经 `cxx` 调 Rust 引擎 |
| rc.1：“实现 Rust 2024 独立解析/验证器” | 无冲突，正是政策方向 | 已在 `wutong-core` 实现 |
| 六语言 SDK 各自实现 | 重复实现协议规则与 Rust 优先矛盾 | Python（PyO3）、TypeScript（wasm）、C++（C ABI）、C#（P/Invoke）均**绑定 Rust 核心**；**Go 例外**：文档要求避免强制 cgo，因此纯 Go 实现，并以跨语言向量 + CI 漂移检查锁定一致性 |
| Python 参考实现（`src/wtp/*.py`）是包内主要“可运行实现” | 不能当作正式后端（rc.1 §20 亦如此说明） | 保留在 `wutong-protocol` 作为测试 oracle；正式 Python 库为 `wutong-sdk-python` |
| Qt 6.12 LTS 基线 | CI 的 Ubuntu apt 仅提供 Qt 6.4；本机为 6.8 | CMake 要求 Qt ≥ 6.4 以便 CI 构建；是否强制 6.12（改用 aqtinstall）需用户决策（Issue） |
| 文档：C++23 | `cxx` 生成代码与 C++17+ 兼容 | GUI 使用 C++23，桥接生成代码按 C++17 编译，互不影响 |

## 7. 需用户决策的文档歧义（均已建 `status: needs-decision` Issue）

1. **消息种类数量**：rc.1 为 15 类，v0.1.2 Schema 为 24 类 → 已按 v0.1.2 机器 Schema 实现 24 类（rc.1 作为历史存档）。
2. **时间戳小数秒**：规范正文允许，Schema 与 Python 参考实现不允许 → 当前实现不允许。
3. **扩展命名空间**：规范要求 ≥3 段反向域名，Schema/Python 接受 2 段 → 当前接受 2 段。
4. **Receipt 状态枚举**：规范有 `rate_limited`/`conflict`，Schema 为 `notarized`；`retry_after` 最小值规范为 1、Schema 为 0。
5. **service_invocation 必需字段**：规范要求 `capability_ids` 1..16，Schema 要求 `idempotency_key`。
6. **Space 可见性**：规范 `local_only`/`federated`，Schema `local`/`private`/`shared`/`public`。
7. **Annotation selector**：Fragment vs Node 命名；`target_revision` 为 sha256 摘要而非 URN。
8. **签发漂移**：规范为绝对值 `|occurred_at − issued_at| ≤ 300`，Python 为单侧 → Rust 验签用绝对值，语义层兼容单侧。
9. **HELLO 示例缺少 `supported_codecs`**：示例 `sync_frame.json` 不满足语义规则（各 SDK 测试中已标注）。
10. **sha256.json 第 6 例无输入字节**，且与 `test_crosslang` 夹具不一致。
11. **审计报告 §5.4 摘要勘误**：报告写 `61dc…`，向量实际为 `3838d5…`。
12. **THIRD_PARTY_NOTICES.md 缺失**；README 曾引用不存在的 `docs/`（已补）。
13. **Qt 6.12 基线 vs CI Qt 6.4**（见 §6）。
14. **nonce 重复但摘要不同**：Python 视为重放，Rust 返回 `ERR_EQUIVOCATION`。
15. **示例 `base_url` 使用看似真实的域名 `node.wutong.space`**：需确认域名归属或改为 `example.org`。
16. **“Wutong Suit” 拼写**：文档中为正式名称，是否应为 “Suite”。

## 8. 项目看板

组织级 Projects v2 看板 **Wutong Roadmap** 收录全部 Issue，字段：Status（Todo / In Progress / Done）、里程碑、仓库、优先级标签。新 Issue 通过模板自动带 `status: triage`。
