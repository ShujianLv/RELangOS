# RELang OS

**面向 AI 代理与多设备协同的操作系统**

> RVM 之于 RELang OS，如同 ART 之于 Android。

本仓库是 **对外宣传与文档入口**（README + [GitHub Pages](https://shujianlv.github.io/RELangOS/)）。  
**完整源码在独立私有仓库维护**，不通过本仓公开。合作、评估与设备适配见文末「源码访问」。

---

## 为什么做 RELang OS

移动与边缘 OS 长期面向「人点 App、单机粗粒度权限」。  
内存安全、端侧 AI、能力模型与多设备协同已经同时成熟，但现有栈很难原生承载：

| 新常态 | 旧栈的摩擦 |
|--------|------------|
| 代理代替人手调用能力 | 权限与工具散落在各 App，难审计 |
| 手机 / 平板 / 车机 / 机载盒一体作业 | IPC 与账号体系按单机设计 |
| 服务要热升级、局部崩溃可恢复 | 粗粒度进程重启成本高 |
| 推理跑在 NPU / GPU，任务跑在监督树里 | 缺少统一的「任务 OS」边界 |

RELang OS 的回答：把 Erlang 验证过的 **隔离进程 · 消息 · 监督树 · 热升级 · 分布式** 提升为系统第一性原则，用 **Rust 实现的 RVM** 跑应用与系统服务。

---

## 一句话定位

应用运行在 **RVM**（Rust BEAM 虚拟机）上；系统服务是被监督的 Actor；权限是不可伪造的 **能力**；AI 代理与现场任务是一等公民，而不是外挂助手 App。

## 与 Android 的对照（对外叙事）

| 层次 | Android | RELang OS |
|------|---------|-----------|
| 应用运行时 | ART | **RVM**（解释 / JIT / AOT） |
| 进程孵化 | Zygote | **Seed**（预热镜像孵化） |
| IPC | Binder | 消息传递 + 域间通道 |
| 系统服务 | system_server | **监督树**中的服务进程 |
| 应用语言 | Java / Kotlin | **Gleam**（主推）、Elixir、Erlang |
| 原生扩展 | NDK | **Wasm**（默认）、受限 Rust |
| 权限 | Manifest + uid | **能力句柄**（可衰减、可审计） |
| UI | View / Compose | 声明式 UI（状态在进程，渲染在 Rust） |

---

## 五大差异化

1. **AI 代理原生** — 代理 = 被监督进程；工具调用 = 消息；权限 = 能力；配额与审计内建；可对外部编程助手暴露 MCP 适配面。  
2. **多设备即一台机器** — 手机、手表、平板、车机、地面站与现场终端组成安全会话集群；迁移与协同走同一套消息语义。  
3. **永不重启（叙事目标）** — 故障优先重启子树；系统组件支持热升级，减少整机重刷。  
4. **结构性安全** — Rust 运行时 + 无环境权限 + 不可信原生代码进隔离域。  
5. **无全局 GC 卡顿** — 每进程独立堆；UI 路径不绑定全局停顿。

---

## 架构鸟瞰（概念层）

```text
应用 / 代理（Gleam · Elixir · Erlang）
        │  消息 · 能力 · Intent
系统监督树（framework 服务）
        │
   RVM（BEAM 语义 · Seed · Dist · Dirty）
        │
原生层：合成器 · UI 引擎 · Agent Bridge · HAL · 加速器运行时
        │
Linux（前期）· cgroup / 异步 I/O / 可选隔离
```

**诚实边界**：不自研芯片级驱动；前期复用主线 Linux / 厂商 BSP。  
首发不与 Android 在通用消费手机上全面对撞，优先 **垂直设备与边缘任务机**。

---

## 垂直场景

### 消费与多设备

启动器、设置、声明式 UI、应用包（`.rpk`）、多设备会话与本地优先同步。适合作为「个人设备集群」的长期故事。

### 端侧 AI

- 系统级代理：工具注册、确认策略、配额、审计  
- 统一推理面：CPU / GPU / NPU / TPU 视为同一加速器类的后端，应用不绑厂商 API 字符串  
- 大块张量走句柄，不塞进邮箱；推理在隔离域 + 后台线程，避免拖垮 UI 帧钟  

### 现场伴飞 / 伴控（FieldCompanion）

无人机伴飞与机器人边缘任务 **共用** 同一垂直 Profile：

| RELang 负责 | 外部栈负责 |
|-------------|------------|
| 作业编排、围栏意图、多机协同、OTA、感知推理 | 飞控内环、伺服周期、安全 PLC |
| `mavlink` / `ros2` 等宿主桥（任务意图） | PX4 / ArduPilot、运动控制器、厂商 SDK |

**不做**：在 RVM 内嵌飞控、EtherCAT 主站或轨迹插补。  
**合适位置**：机载伴飞电脑、工业臂旁智能盒、仓内 AMR / 巡检机器人、地面站与编队协同。

---

## 设计原则（摘要）

1. 一切皆进程，一切皆消息  
2. 任其崩溃，结构化恢复（监督树）  
3. 无环境权限：只能凭被授予的能力句柄做事  
4. 信任边界分层：语言隔离 → 域 / 进程 → 硬件隔离  
5. 位置透明：本地与远端同一套消息语义  
6. 确定性可测：时间、随机、I/O 可注入，便于回放与审计  
7. 能耗是一等公民；务实复用成熟驱动与编解码  

---

## 开发者会接触到什么（公开口径）

| 角色 | 表面 |
|------|------|
| 应用开发者 | Gleam UI / framework 包、声明式 Screen、工具 schema |
| 系统集成 | 能力、Domain、HAL 发现（出现 ≠ 可用）、诚实失败（`unavailable`） |
| 外部 AI 编程助手 | MCP / LSP / 预览适配（Host 策略下）；**不自研 Codex** |
| 现场设备 | FieldCompanion Profile：任务服务 + 控制面桥 + mesh 地面站 |

---

## 在线站点

**https://shujianlv.github.io/RELangOS/**

（仓库 Settings → Pages → `main` / `/docs` 启用后生效。）

---

## 源码访问

- 本仓 **不含** RVM、framework、HAL、SDK 实现。  
- 评估、合作、OEM / ODM 适配：请开 [Issues](https://github.com/ShujianLv/RELangOS/issues) 或联系 maintainer 申请 **私有源码仓** 只读权限。  
- 宣传文案许可见 [LICENSE](LICENSE)（MIT）；产品源码许可以私有仓声明为准。

## 本仓库结构

```text
README.md           本页
docs/index.html     GitHub Pages 主页（更完整的叙事页）
docs/setup.md       双仓与 Pages 维护说明
LICENSE             宣传仓 MIT
```

---

### English

**RELang OS** is an AI-native, multi-device operating system whose apps run on **RVM**, a Rust implementation of the BEAM VM. Agents, capabilities, supervision trees, and field companion workloads (drones / robots as *mission* OS, not flight/servo loops) are first-class. This repository is the **public face only**; implementation source remains **private**.
