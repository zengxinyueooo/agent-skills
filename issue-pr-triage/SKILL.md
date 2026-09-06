---
name: issue-pr-triage
description: 输入一个 GitHub issue,产出可执行的实现计划——问题理解、代码定位、难度评估、查重、分步实现方案与风险。Use when the user gives an issue URL / owner-repo#number and wants to decide whether to claim it or prepare to work on it. Requires gh CLI. Do not use for auto-writing patches, auto-opening PRs, or triaging without repository read access.
---

# Issue / PR Triage

把一个 issue 变成一份能直接动手的计划,服务开源贡献的第一公里——最难的不是写代码,是"这个 issue 值不值得我碰、从哪个文件下手"。

## Start Gate

- issue 定位:URL 或 `owner/repo#编号`;
- `gh` CLI 已登录且能读该仓库;
- 用户说明意图:评估要不要认领 / 已决定要修。

意图为"批量扫一遍仓库所有 issue"→ 建议先限制 3-5 个再逐个跑,不做全仓扫。

## Workflow

1. **理解问题**:读 issue 正文与全部评论,提取:期望行为 / 实际行为 / 复现步骤 / 环境信息;信息不足时列出"该问 reporter 的问题清单"(这本身就可以贴到 issue 下);
2. **定位代码路径**:从报错关键词、报错堆栈、模块名在仓库内检索,给出 likely files + 每个文件为什么相关 `[来源: 路径:行号]`;
3. **查重**:`gh` 搜索该仓库关联的 issue / PR(open 与 closed);已有人在做 → 给链接 + 判断进展 + 给出"还值不值得做"的建议;
4. **难度评估**:改动面(单文件 / 跨模块 / 需要设计)+ 需要的领域知识 → `easy / medium / hard` + 理由;
5. **实现计划**:分步(改哪里 / 加什么测试 / 是否动公共接口),风险点(兼容性 / 性能 / 安全),预估规模。

## Output Contract

固定七段:

```
Difficulty: <easy|medium|hard> — <理由>
Likely files:
  - <路径> — <为什么相关>
Existing work: <无 / issue# / PR# + 状态>
Implementation:
  1. <步骤>
Risk: <兼容性/性能/安全,每条一句>
Estimated scope: ~<N> LOC
Verdict: <值得认领 / 观望 / 放弃> — <一句话>
```

## Boundaries

- 只读分析 + 搜索,不自动写补丁、不自动开 PR、不自动评论 issue;
- likely files 是假设不是结论,动手前必须实际打开验证;
- 维护者已表态方向的(评论/labels),计划必须遵循,不另起炉灶;
- Estimated scope 给量级(~300 LOC),不给假精确(287 LOC);
- 建议配合 repo-onboarding 使用:不熟悉的仓库先做 onboarding 再 triage。
