# onboarding-AgentRecall-20260908

> 本文件是 [repo-onboarding](../../SKILL.md) 的**真实运行示例**:目的 = 学架构 + 准备贡献;调研方式 = GitHub API 只读(未 clone,代码级结论待本地验证);生成于 2026-09-08。脚注路径均可溯源,未核实处如实标注。

## Repository Overview

AgentRecall —— 本地 Electron 桌面应用:索引、展示、恢复 AI Coding Agent 会话(Claude Code / Codex 等)。

- monorepo 双应用:V1(stable,SQLite + 同步 stores)与 V2(preview,PostgreSQL + 异步 stores,新增 Runtime / Agents / Chat / Workflow / Eval / MCP / Memory / 托管 Skill 库)[来源: AGENTS.md]
- 技术栈:Electron + React 19 + TypeScript + Vitest,Node ≥22.13 [来源: package.json, apps/main-2.0/package.json]
- MIT 协议,★763,创建于 2026-06,最近推送 2026-09-07(活跃) [来源: repo metadata API]
- 贡献指南为中文,要求 PR 前先 Issue 沟通 [来源: CONTRIBUTING.md]

## Architecture

进程内三段式 + 领域层(两个 app 同构):

```
src/main/       Electron 主进程:服务、IPC handlers、OS 集成
src/preload/    窄类型桥(暴露给渲染进程的唯一通道)
src/renderer/   React UI,只允许浏览器安全代码
src/core/       领域逻辑、加载器、持久化、共享应用类型
src/automation/ 仅 V2:Agent / Chat / Workflow / Eval / 运行时引擎
src/mcp|shared/ 仅 V2:MCP 集成 / 共享代码
```
[来源: AGENTS.md「Repository map」+ apps/main-2.0/src 实际目录核对]

**分层纪律**(对贡献者是最重要的隐性规范):改动留在最低归属层;V2-only 行为不得进入共享的 V1 代码;展示逻辑不得进入持久化模块;OS/文件系统操作不得出现在 React 组件。[来源: AGENTS.md]

数据面差异是 V1/V2 的主轴线:V1 = SQLite + mostly synchronous stores,V2 = PostgreSQL + async stores。[来源: AGENTS.md]

## Core Modules

| 模块 | 职责 | 备注 |
|---|---|---|
| apps/main-1.0 | 稳定版产品 | [来源: AGENTS.md] |
| apps/main-2.0 | 预览版产品(升级会话体验 + 全部新引擎) | [来源: AGENTS.md] |
| scripts/ | 仓库 setup、release-note 校验、打包冒烟、发布检查脚本(带独立单测 `*.test.mjs`) | [来源: package.json scripts, 根目录列表] |
| docs/ | 用户指南、排障、持久设计文档 | [来源: 根目录列表;内容未逐篇核实] |
| .release-notes/ | 用户侧发布说明片段,由发布工作流消费(有格式校验命令) | [来源: AGENTS.md, package.json] |

## Entry Points

- 开发:`npm run setup:v1|v2` → `npm run dev:v1|v2` [来源: AGENTS.md「Commands」]
- 迭代时跑聚焦测试:`npm exec vitest run <file>`(cwd = apps/main-2.0);聚焦通过后才跑 app 级 test/typecheck;**不默认跑全仓 suite**(留给全仓变更 / CI 诊断 / 发布准备) [来源: AGENTS.md]
- 构建入口:apps/main-2.0/electron.vite.config.ts [来源: 目录列表]

## Important Files

- `AGENTS.md` — 仓库地图、分层规则、命令纪律,贡献前必读 [来源: AGENTS.md]
- `CONTRIBUTING.md` — Issue/PR 流程与模板要求 [来源: CONTRIBUTING.md]
- `apps/main-2.0/src/automation/` — V2 差异化的核心(本调研未深入,clone 后优先读)[标注: unknown]
- `THIRD_PARTY_NOTICES.md` — V2 的第三方声明,动依赖前看 [来源: 文件存在,内容未核实]
- `docs/` — 「持久设计文档」:理解设计意图的第二入口 [来源: AGENTS.md;篇目未核实]

## Contribution Rules

1. PR 前必须先开 Issue 对齐方案;Bug 走模板(环境 / 复现步骤 / 预期 vs 实际 / 日志),Feature 走模板(问题与场景 / 可选方案)[来源: CONTRIBUTING.md]
2. 提 Issue 前先搜 Issues 与 Discussions 查重 [来源: CONTRIBUTING.md]
3. 测试纪律见 Entry Points;`release:preflight` 昂贵,勿随手跑 [来源: AGENTS.md]
4. 发布说明以 `.release-notes/` 片段提交,格式由 `npm run release-note:check` 强制 [来源: AGENTS.md, package.json]

## Suggested Starter Tasks

1. **本地跑通 V2**:`setup:v2` + `dev:v2`,把体验问题/文档不符处记下来——文档类 PR 的天然素材 [推断,依据: 双 app 结构 + docs/ 定位]
2. **读 `docs/` 设计文档并对照 `src/automation/`**:「持久设计文档 vs 实现」的不一致即高质量 PR 来源 [推断;automation 内部结构本次未核实,标注 unknown]
3. ** watch `.release-notes/` 学习变更粒度**:发版频繁(2026-06 建库至今),从 release note 反推模块边界是低成本学习路径 [推断,依据: pushed_at 与 release 工作流存在]
4. 当前 open issue 仅 2 个且无 `good first issue` 标签 → **不建议盲抢 issue,走「Issue 先沟通方案」路径更现实** [来源: issue 搜索 API]

## Grounded 声明

本报告基于 GitHub API 只读调研(元信息 / 根目录 / AGENTS.md / CONTRIBUTING.md / package.json / apps/main-2.0 目录),未 clone 仓库、未执行代码;所有「推断」标注的条目需本地验证后才可作为行动依据。
