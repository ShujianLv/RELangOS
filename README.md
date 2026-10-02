# RELang OS

**面向 AI 代理与多设备协同的新一代操作系统** —— RVM 之于 RELang OS，如同 ART 之于 Android。

本仓库是 **对外宣传与文档入口**（README + GitHub Pages）。**完整源码在独立私有仓库维护**，不通过本仓公开；合作与评估请见下方「源码访问」。

---

## 一句话

把 Erlang 验证过的 **隔离进程 · 消息传递 · 监督树 · 热升级 · 分布式** 提升为 OS 第一性原则；应用运行在 Rust 实现的 **RVM**（BEAM 虚拟机）上，系统服务与 UI 栈以内存安全的 Rust 为主路径。

## 差异化

| 方向 | 说明 |
|------|------|
| AI 代理原生 | 代理 = 被监督进程；工具 = 消息 + 能力 + 审计 |
| 多设备即一台机器 | 手机 / 平板 / 车机 / 现场终端组成安全 RVM 集群 |
| 永不重启 | 子树恢复、系统组件热升级 |
| 能力安全 | 无环境权限；不可伪造的能力句柄 |
| 垂直设备优先 | 工业手持、AI 硬件、无人机伴飞、机器人边缘任务等 |

## 垂直场景（架构定位）

- **消费与多设备**：启动器、设置、120Hz UI 栈、Gleam 应用包（`.rpk`）
- **现场伴飞 / 伴控（FieldCompanion）**：机载/边缘 **任务与协同 OS**；通过 MAVLink / ROS 2 等桥接外部飞控或运动控制，**不替代**硬实时内环
- **端侧 AI**：统一加速器 ABI（CPU / GPU / NPU / TPU）；推理在隔离域运行

## 在线介绍

启用 GitHub Pages 后，站点根路径为：

**https://shujianlv.github.io/RELangOS/**

（仓库名或 Pages 配置变更时请同步修改 [`docs/index.html`](docs/index.html) 中的链接。）

## 源码访问

- 本仓 **不包含** RVM / framework / HAL 等实现代码。
- 评估、合作或设备适配：请通过 GitHub Issues（本仓）或你方已知的 maintainer 联系渠道申请私有仓只读访问。

## 仓库结构

```text
README.md           本页（GitHub 仓库首页）
docs/index.html     GitHub Pages 静态主页
docs/setup.md       维护者：宣传仓 + 私有源码仓 搭建说明
LICENSE             本宣传仓内容许可（MIT）
```

## 许可

本宣传仓中的文案与静态页采用 [MIT](LICENSE)。RELang OS **产品源码**的许可以私有仓库中的 `LICENSE` 为准（对外通常为 Apache-2.0 等，以实际声明为准）。

---

English summary: RELang OS is a mobile/edge OS with **RVM** (Rust BEAM VM), capability security, and native AI-agent integration. This repo is **marketing only**; source lives in a **private** repository.
