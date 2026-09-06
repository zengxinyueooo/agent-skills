---
name: repo-onboarding
description: 输入一个代码仓库,产出带来源标注的结构化 onboarding 报告——架构、核心模块、数据流、开发规范、推荐切入点。Use when the user wants to understand a repo (own contribution, learning architecture, or tech evaluation) and the repo is cloned locally or accessible via gh. Do not use for executing repo code, security audits, or generating reports without reading source files.
---

# Repo Onboarding

输入一个仓库,输出一份下次还能接着用的结构化报告。

同类工具的产出常常是"README 复读机"——这个 skill 的生命线是一条铁律:**每个结论标注来源文件路径,没读到代码就明说**。

## Start Gate

- 仓库来源:本地 clone 路径,或 gh 可访问的 GitHub 仓库(只读 API);
- 用户声明阅读目的:准备贡献 / 学架构 / 选型评估——决定报告的深读重点;
- 超大仓库(>500 文件):先与用户圈定 1-2 个深读模块,不贪全。

## Workflow

1. **元信息层**:README / AGENTS.md / CLAUDE.md / CONTRIBUTING.md / LICENSE / CI 配置;目录树(限深度 3);语言构成;
2. **核心模块定位**:从入口文件(main / cli / index / app)出发,沿 import/调用关系圈出 3-5 个核心模块,判据 = 被引用最多 ∪ 名字最像核心域;
3. **数据流走查**:挑一条最有代表性的链路(如"一次用户请求从入口到落库"),文字版时序描述,每步标函数所在文件;
4. **开发规范归纳**:CONTRIBUTING + 代码风格约定 + commit 历史的惯用格式 + CI 强制项;
5. **切入点推荐**:good-first-issue / TODO 注释 / 明显缺测试的模块 / 文档与代码不一致处,各给"为什么适合作为第一步"。

## Output Contract

`onboarding-<repo>-<YYYYMMDD>.md`,固定七节:

```
## Repository Overview
## Architecture          <模块关系,文字版架构图>
## Core Modules          <每个:职责 / 入口 / 关键依赖>
## Entry Points
## Important Files       <每条:路径 — 为什么重要>
## Contribution Rules
## Suggested Starter Tasks
```

每条结论后缀 `[来源: 路径]`;仅 README 佐证的标 `[来源: README,未验证]`;没查到的标 `unknown`。

## Boundaries

- 必须读到代码本身;只总结 README 的产出不合格;
- 只读分析:不执行仓库代码、不装依赖;构建/测试需用户显式确认;
- 来源标注不可省略——这份报告的价值就在于一个月后还能溯源;
- 报告落盘到用户的笔记目录,不进被分析仓库。
