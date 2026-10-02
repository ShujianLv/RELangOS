# RELang OS

**为代理与协同而生的操作系统。**

应用运行在 **RVM** 上 — Rust 实现的 BEAM 虚拟机。RVM 之于 RELang OS，如同 ART 之于 Android。

→ **官网式介绍（推荐）**：[https://shujianlv.github.io/RELangOS/](https://shujianlv.github.io/RELangOS/)  
→ **源码**：独立私有仓库，不在本仓公开 · [申请访问](https://github.com/ShujianLv/RELangOS/issues)

---

## 它在解决什么

移动与边缘系统长期按「人点 App、单机、粗粒度权限」建造。当设备开始替人调用工具、跨端完成一件事、在野外局部故障后自我恢复时，这些假设会先碎裂。

RELang OS 的回答不是再叠一层助手 App，而是换一套系统原语：

**一切皆进程，一切皆消息。** 错误靠监督树结构化恢复。能做的事只能来自被授予的能力句柄 — 没有环境权限。本地与远端使用同一套消息语义。设备不在、宿主未挂时诚实失败，不编造成功。

这把 Erlang 验证过的模型，从应用库提升为操作系统的第一性原则；用 Rust 保证运行时本身的内存安全与可嵌入性。

---

## 原理（四条）

| | |
|--|--|
| **任其崩溃，结构化恢复** | 优先重启子树；组件可热升级，减少整机重刷依赖。 |
| **没有环境权限** | 不能凭名字拿到相机或电台。能力可衰减、过期、审计；代理与应用同一套闸。 |
| **位置透明** | 另一台设备上的 Actor 与本机使用同一消息语义。多设备是运行时能力，不是账号附属。 |
| **诚实失败** | `unavailable` 优于假邻居、假起飞、假推理。 |

更完整的叙事与排版见 [站点](https://shujianlv.github.io/RELangOS/#idea)。

---

## 架构（一层运行时贯通）

```text
Gleam / Elixir / Erlang 应用与代理
        │  消息 · Intent · 能力
系统监督树（framework）
        │
RVM — 解释 / JIT / AOT · Seed · Dist · Dirty
        │
原生层：合成器 · UI 引擎 · HAL · 加速器运行时
        │
Linux（前期）· 隔离 · 异步 I/O
```

开发者面对统一的 RVM 与能力模型；换设备时换 HAL 与产品 Profile。UI 状态在进程里，像素路径在 Rust — 避免把帧钟绑在全局 GC 上。厂商 SDK 与不可信原生进 Isolated；长推理进 Dirty，不堵调度器。

内核前期复用主线 Linux / 厂商 BSP：不自研芯片级驱动，创新集中在运行时与框架。

---

## 优势与价值

**给产品与交付** — 垂直设备可以卖「可恢复、可审计的代理作业、多终端一体」，不必先赢消费应用商店。

**给系统集成** — Domain 与能力把崩溃域、权限与原生桥边界写死；缺硬件时诚实失败，联调不靠假成功。

**给现场与机队** — 任务软件可热升级；编队与地面站走同一 mesh 叙事，而不是临时拼脚本。

窗口期在架构：端侧模型、NPU、代理协议与内存安全语言同时就绪，而旧栈仍以 App 为粗粒度单位。RELang 以进程、能力与代理为粒度。

---

## 特性（系统一等公民）

- **AI 代理原生** — 代理是被监督进程；工具 = 消息；权限 = 能力；配额与审计内建；Host 可对接 MCP / LSP（不自研 Codex）。
- **多设备即一台机器** — 发现、配对、会话、迁移有明确状态；未连通不假装连通。
- **加速器统一面** — CPU / GPU / NPU / TPU 同一 `AcceleratorClass`；大张量走句柄；与 UI 绘制队列隔离。
- **声明式 UI · 原生渲染** — Gleam 等写状态与业务；合成与上屏在 Rust。
- **可测可回放** — 时间 / 随机 / 输入可注入；利于代理审计与现场追溯。

---

## 垂直：FieldCompanion

无人机伴飞与机器人边缘任务共用同一 Profile — **做任务 OS，不当飞控 / 伺服内环**。

| RELang | 外部栈 |
|--------|--------|
| 作业编排、感知闭环、围栏与安全意图、mesh、OTA | 姿态环、伺服周期、安全 PLC |
| MAVLink / ROS 2 等宿主桥 | PX4、ArduPilot、运动控制器、厂商 SDK |

合适：机载伴飞电脑、地面站与编队、AMR / 巡检、臂旁智能盒。  
不合适：取代飞控固件、EtherCAT 主站、安全 PLC。

---

## 与 Android 的对照（沟通地图，非生态对撞）

| 层次 | Android | RELang OS |
|------|---------|-----------|
| 运行时 | ART | RVM |
| 孵化 | Zygote | Seed |
| IPC | Binder | 消息 + 域间通道 |
| 服务 | system_server | 监督树服务 |
| 语言 | Java / Kotlin | Gleam（主）/ Elixir / Erlang |
| 扩展 | NDK | Wasm（默认） |
| 权限 | Manifest + uid | 能力句柄 |
| UI | View / Compose | 声明式 · Rust 渲染 |

**非目标**：首发不做通用消费手机全面竞争；不追求 100% OTP；不自研芯片驱动。

---

## 本仓库

宣传入口 only。站点源文件在 [`docs/index.html`](docs/index.html)；维护说明见 [`docs/setup.md`](docs/setup.md)。

| | |
|--|--|
| 宣传仓 | Public · 本仓 |
| 产品源码 | Private · 另仓 |

合作、OEM / ODM、运行时评估：请开 [Issues](https://github.com/ShujianLv/RELangOS/issues)。

宣传文案 [MIT](LICENSE)；产品源码许可以私有仓为准。

---

### English

RELang OS is built around supervision trees, capabilities, and a Rust BEAM VM (**RVM**). Agents and multi-device collaboration are first-class; FieldCompanion covers drone/robot *mission* workloads without replacing flight/servo loops. This repo is the public narrative; source access is granted privately.
