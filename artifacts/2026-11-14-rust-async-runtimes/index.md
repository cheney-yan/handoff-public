---
title: 2026 年 Rust 异步运行时现状：tokio、async-std、smol 怎么选
---

# 2026 年 Rust 异步运行时现状：tokio、async-std、smol 怎么选

> 数据于 2026-11-14 直接从 crates.io API 和 GitHub API 拉取；下载量包含传递依赖和 CI，只能当粗略信号。标「我的判断」的是观点。

## 一句话

**"三选一"已经不成立了：async-std 已停更（官方叫大家迁到 smol），tokio 是事实标准，smol 是轻量派的唯一正统继承人；真正的新分歧在 tokio 和 io_uring 系的 thread-per-core 运行时（compio 等）之间。**

## 要点

1. **async-std：已停更。** 官方 README 写着 "async-std has been discontinued; use smol instead"（2025-03 宣布），RustSec 发了 [RUSTSEC-2025-0052](https://rustsec.org/advisories/RUSTSEC-2025-0052.html)（unmaintained，2025-08）。最后版本 1.13.2（2025-08-15），仓库自那以后无提交。还能编译能跑，但没人修新问题。
2. **tokio：一家独大，而且很稳。** 最新 1.54.1（2026-10-21），差不多每月一个 minor；有 LTS 线（1.51.x 支持到 2027-03，1.53.x 到 2027-09）。近 90 天下载约 3.08 亿次，大约是 smol 的 39 倍。
3. **smol：活着，但"小"是故意的。** smol 本体 2.0.2 停在 2024-09，但仓库还在维护（2026-10 有提交）。真正活跃的是它拆开的零件：async-executor 1.14.1（2026-11-02）、async-io 2.6.0。这些零件的下载量远高于 smol 本身，说明大量项目是按零件用的。
4. **性能：日常场景差别不大，差别主要在 I/O 模型。** 普通网络服务里 tokio 的 work-stealing 调度已经够好。真正拉开差距的是磁盘 I/O 很重、可以按核分片的系统（数据库、消息队列）：Apache Iggy 从 tokio 迁到 compio（io_uring、thread-per-core）后，在他们自己的压测里尾延迟（P99.9/P99.99）改善了 20%–90%。这是厂商自测的数字。
5. **2026 新动态：** tokio 1.54 把 MSRV 提到 Rust 1.85，新增 wasm32-unknown-emscripten 支持，稳定了 `worker_index()`；io_uring 系里 compio 最活跃（0.19.2，2026-09），monoio 迁到 monoio-rs 组织后还有维护但跟进 io_uring 的速度慢，glommio 基本没在动（0.9.0 停在 2024-03）。
6. **对王五爬虫问题的直接答案：** rquest 已经改名为 **wreq**（最新 0.16.3，2026-10-30），依赖 tokio + hyper。用它基本就是绑定 tokio，没得选。

## 硬数字（2026-11-14 拉取）

- **tokio**：最新 1.54.1（2026-10-21）· 总下载约 11.9 亿 · 近 90 天约 3.08 亿 · GitHub ★33.7k · open issues/PR 445 · 最近 push 2026-11-12
- **async-std**：最新 1.13.2（2025-08-15）· 总下载约 1.01 亿 · 近 90 天约 1195 万（基本是传递依赖）· ★4.1k · 最近 push 2025-08-15（已停更）
- **smol**：最新 2.0.2（2024-09-07）· 总下载约 2867 万 · 近 90 天约 795 万 · ★5.1k · 最近 push 2026-10-26
- **async-executor（smol 零件）**：1.14.1（2026-11-02）· 近 90 天约 4614 万
- **async-io（smol 零件）**：2.6.0（2025-09-14）· 近 90 天约 5754 万
- **compio**：0.19.2（2026-09-29）· 近 90 天约 41 万 · ★2.0k · 最近 push 2026-11-13
- **monoio**：0.2.4（2024-08-20）· 近 90 天约 41 万 · ★5.1k · 最近 push 2026-10-16
- **glommio**：0.9.0（2024-03-25）· 近 90 天约 2.3 万 · ★3.7k

## 逐个讲

### tokio：默认选项，生态替你选了

- **机制：** 多线程 work-stealing 调度，基于 epoll/kqueue/IOCP 的就绪通知（mio）。网络、文件、进程、信号、同步原语、定时器全都有。
- **生态：** axum、tonic、hyper、reqwest、wreq、sqlx、tokio-tungstenite……主流 Web/网络库默认都用 tokio。hyper 自己有一层 `hyper::rt` 抽象，理论上能跑在别的运行时上，但实际部署几乎全是 tokio。Cloudflare 的 Pingora 也基于 tokio。
- **短板：** 文件 I/O 其实是丢给阻塞线程池做的（默认最多 512 个线程），因为 epoll 对普通文件总是返回"就绪"。磁盘密集的系统会在这里撞墙。另外 `Send + 'static` 的任务约束、编译体积和 `full` feature 的依赖量也常被吐槽。
- **稳定性承诺：** 有滚动 MSRV（至少 6 个月），还有 LTS 线，这对生产环境很重要。

### async-std：历史功臣，已退役

- 2019 年推出，API 刻意照抄 std，那时是 tokio 之外唯一像样的选择（tide、surf 都跑在它上面）。
- 后来它内部本身就是基于 smol 的零件构建的，维护者干脆宣布停更，推荐迁到 smol。
- **怎么处理：** 新项目别用。老项目跑 `cargo audit` 会一直报 RUSTSEC-2025-0052（默认只是警告）。不急，但该排期迁移。

### smol：小而透明

- **机制：** smol 本身几乎只是 re-export，真正干活的是 async-executor、async-io、blocking、async-net、async-fs 这些零件。没有过程宏，`smol::block_on` 用起来非常直白，代码量小，读得懂。
- **适用：** 想自己控制 executor 的场景（嵌入别的事件循环、GUI 应用、CLI 工具），想要依赖少的库，或者需要多个小 executor 的场景。要用 tokio 生态的库时，可以通过 `async-compat` 桥接。
- **注意：** "smol 本体两年没发版"并不等于死了。零件一直在发版，仓库一直有维护。但它的生态体量和 tokio 不在一个数量级。

### 第四类：io_uring / thread-per-core（compio、monoio、glommio）

- **机制：** 每核一个单线程 executor，任务不跨核迁移（不要求 `Send`），I/O 走完成式接口（io_uring / IOCP），磁盘 I/O 真正异步。代价是负载均衡得自己做，生态小，只能用自家的 TCP/文件类型。
- **Apache Iggy 的实测（2026-02 博客）：** 三个都试过了。monoio 跟进 io_uring 新特性慢；glommio "基本不维护了"（作者 Glauber Costa 去了 Turso）；最后选了 compio，理由是维护活跃（补丁几小时就合并）、driver 和 executor 解耦、io_uring 覆盖面广。代价是每个请求一次小的堆分配。
- **适用：** 数据库、消息队列、存储、代理这类能按核分片的 Linux 服务。普通业务服务没必要。

## 怎么选（我的判断）

1. **Web 服务 / gRPC / 爬虫 / 用 reqwest 或 wreq：** 直接 tokio。生态已经替你决定了，别为了"轻量"去和整个生态较劲。
2. **写库：** 尽量做到运行时无关（只依赖 `futures` 的 trait，或者把 spawn/timer 交给调用方注入）。实在要选，默认支持 tokio。
3. **CLI、桌面/GUI、嵌入式 Linux 小工具，或者对依赖量敏感：** smol 很合适，代码少、心智负担小。
4. **磁盘 I/O 很重、可以按核分片的系统：** 先评估 compio。glommio/monoio 现在不建议新上。（Embedded 无 OS 场景另有 embassy，不在本文讨论范围。）
5. **还在用 async-std 的：** 排期迁移。大多数情况迁到 smol 改动最小，如果依赖本来就在 tokio 生态，就迁 tokio。

## 未核实 / 存疑

- 性能对比没有权威的中立基准。各家博客的数字差异很大，Iggy 的数据是他们自己的工作负载。普通网络服务里 tokio 和 smol 谁快，取决于负载形态，本文不给结论。
- 下载量里包含传递依赖和 CI。async-std 近 90 天还有约 1200 万下载，主要应该是老依赖链拉进来的（这是推测）。
- "Rust 标准库会内置 executor / 运行时无关的 async trait 什么时候落地"：截至本文没有查到确定时间表。

## 来源

- async-std README（停更声明）：https://github.com/async-rs/async-std （2025）
- RUSTSEC-2025-0052 async-std is unmaintained：https://rustsec.org/advisories/RUSTSEC-2025-0052.html （2025-08）
- tokio README（MSRV / LTS / 发布节奏）：https://github.com/tokio-rs/tokio （2026-11 读取）
- tokio 1.54.0 release notes：https://github.com/tokio-rs/tokio/releases/tag/tokio-1.54.0 （2026-10-19）
- smol 仓库：https://github.com/smol-rs/smol
- crates.io API：https://crates.io/api/v1/crates/{tokio,smol,async-std,async-executor,async-io,compio,monoio,glommio,wreq} （2026-11-14 拉取）
- Apache Iggy, "migration journey to thread-per-core io_uring"：https://iggy.apache.org/blogs/2026/02/27/thread-per-core-io_uring/ （2026-02-27）
- Rust Trends, "Rust Async Runtimes Compared: Tokio vs Smol in 2026"：https://rust-trends.com/posts/rust-async-runtimes-compared/
- corrode.dev, "The State of Async Rust: Runtimes"：https://corrode.dev/blog/async/
- wreq（原 rquest）：https://github.com/0x676e67/wreq

[← 返回首页](../../)
