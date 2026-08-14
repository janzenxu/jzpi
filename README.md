# JZPI

JZPI 是基于开源项目 [agegr/pi-web](https://github.com/agegr/pi-web) fork 的个人版 **pi coding agent 工作台**，同时也是一个通过真实项目演进学习 Web、Agent SDK 与工程架构的长期学习项目。

项目当前处于 fork 整理与架构收敛阶段。现阶段首先保持 pi-web 已有能力稳定可用，再逐步在 WebUI 与 pi SDK/raw pi agent 之间建立 JZPI 自己的中间层，用于承载个人工作流、策略和插件编排。

> JZPI 不是 pi core 或 pi TUI 的定制发行版，也不会通过长期修改全局 `node_modules` 来实现功能。

## 项目目标

目标运行形态：

```text
Windows PWA / Edge App
        ↓
浏览器 UI
        ↓  localhost / WSL 端口转发
WSL/Linux 内的 JZPI（默认端口 30142）
        ↓
JZPI 中间层 / 插件编排层
        ↓
pi SDK / raw pi agent
```

核心原则：

- 不 patch pi core
- 不 patch pi TUI
- 不长期修改 npm 全局 `node_modules`
- 不把 WebUI 或个人工作流逻辑塞进 pi TUI extension
- raw pi agent 保持干净、可独立升级和使用
- WebUI 负责交互，中间层负责策略、编排和兼容
- 尽量保持 pi 原生配置、会话格式与工具生态兼容
- JZPI 私有元数据不污染 pi 原生会话数据

项目设计资料在本地工作区的 `.auxiliary/` 中按职责持续维护：

- `BLUEPRINT.md`：宏观设计
- `SCAFFOLD.md`：技术架构与代码落地定义
- `ROADMAP.md`：实现进度与短期规划
- `LOGBOOK.md`：阶段修改记录
- `RUNBOOK.md`：简要使用手册

## 当前状态

当前代码基本保持 pi-web 的实现方式：Next.js API route 在同一 Node.js 进程内直接创建和管理 pi `AgentSession`。规划中的 JZPI 中间层尚未完成，不能把目标架构误认为当前已经实现。

继承自 pi-web 的主要能力包括：

- 浏览、恢复、分支、导出和删除 pi 会话
- Agent 实时执行、SSE 事件流和断线恢复
- 模型、Provider、API Key、OAuth、插件和 Skill 管理
- 项目文件浏览、预览、上传、Git Diff 和 worktree
- PWA、移动端布局、英文和简体中文界面

## 本地开发

要求 Node.js `22.19.0` 或更高版本。

```bash
npm install
npm run dev
```

访问：<http://127.0.0.1:30142>

JZPI 使用 `30142`，以便原版 pi-web 继续使用 `30141`。如需临时指定其他端口：

```bash
npx next dev -H 127.0.0.1 -p 8080
```

常用检查：

```bash
npm test
node_modules/.bin/tsc --noEmit
npm run lint
```

日常开发不要运行 `next build` 或 `npm run build`；构建会写入 `.next/`，可能干扰开发服务器。

## WSL / Windows 使用

推荐在 WSL/Linux 中运行 JZPI，在 Windows Edge 中将其安装为 PWA。默认仅监听 `127.0.0.1`，避免意外暴露可执行高权限操作的 Agent 服务。

如果当前 WSL 网络模式无法从 Windows 直接访问 `127.0.0.1:30142`，应通过 WSL/Windows 的端口转发解决，而不是默认将服务暴露到局域网。确需监听非回环地址时可运行：

```bash
PI_WEB_PASSWORD='足够长的随机密码' npm run dev:lan
```

`PI_WEB_*` 环境变量是从 pi-web 继承的兼容接口，现阶段保留，后续如引入 `JZPI_*` 会提供迁移策略。

## 数据与兼容性

- pi 数据默认位于 `~/.pi/agent`，JZPI 与 raw pi 共用模型配置、凭据和原生会话文件。
- 可通过 `PI_CODING_AGENT_DIR` 指定其他 pi agent 数据目录。
- JZPI 必须运行在能够访问会话 CWD 的文件系统环境中，因此推荐与 raw pi 一同运行在 WSL/Linux。
- 文件浏览 API 有允许目录边界，不是通用文件管理器。
- 未来 JZPI 自有状态应写入独立目录，默认规划为 `~/.jzpi`；在相关存储模块落地前，不向 pi 会话格式写入私有字段。

## Fork 与许可

JZPI 基于 MIT License 的 pi-web 开发，保留原项目版权和许可信息。上游同步应通过 Git 和依赖升级完成，不直接修改 npm 全局安装目录。相关工程约束维护在 `.auxiliary/SCAFFOLD.md`。

## License

[MIT](./LICENSE)
