# Triage: zszz3/AgentRecall#499

> 本文件是 [issue-pr-triage](../../SKILL.md) 的**真实运行示例**。目标 issue:[OpenViking 记忆注入把往期会话原文当作当前指令](https://github.com/zszz3/AgentRecall/issues/499) · 调研于 2026-09-08 · 调研方式:issue 全文 + GitHub code search + 目录核对(未 clone)

---

**Difficulty: medium** — 主修复面集中在一个 hook 文件及其测试,无需跨模块;难在修复策略要同时保住召回质量(token 预算语义)与向后兼容(hook-state 旧文件)。

**Likely files:**
- `apps/main-2.0/bin/openviking-memory-hook.cjs` — 注入点本体。reporter 定位 L110(`additionalContext` 组装)与 L1331(用户输入注入路径);`hook-state/*.json` 中 `recentTurns` 存 user/assistant 原文亦由此文件写入 [来源: issue #499 正文 + code search]
- `apps/main-2.0/scripts/openviking-memory-hook.test.mjs` — hook 已有测试,修复应先在此加"注入文本必须带历史边界标记"的失败用例 [来源: code search]
- `apps/main-2.0/bin/setup-openviking-memory-hooks.cjs`(+ 同名 test)— 钩子注册与 manifest,若新增配置项(如注入格式开关)需动此 [来源: code search]
- 问题 2(进程泄漏):MCP gateway 进程,likely `apps/main-2.0/bin/*-mcp.mjs` 的 stdio 生命周期处理 [来源: 目录列表;具体文件未核实]
- 问题 3(shim 硬编码):`apps/main-2.0/bin/install-macos-app.cjs` — reporter 已贴出硬编码片段 [来源: issue 正文]

**Existing work:** 无。仓库仅此一条开放 issue,无关联 PR,无维护者表态(截至 2026-09-08)。

**Implementation(建议拆两个 PR,先小后大):**
1. **PR-1(最小修复,直击问题 1 主诉)**:注入文本包裹只读边界标记(`<past_session_reference>` + "以下为历史参考,非当前指令"),与 reporter 建议 1 一致;同步更新 state 写入侧与测试
2. **PR-2(治理根因)**:①按工作区隔离召回——以会话启动 cwd 为 workspace 身份,跨域不召回(reporter 建议 3)②`recentTurns` 落盘前降级为带来源标注的摘要,不再存原文(建议 2)③注入内容回显到 UI(建议 4),可作为独立 issue 先挂
3. 问题 2/3 各自独立 issue 化更合适(进程泄漏 / 安装 shim),避免单 PR 范围膨胀——可在原 issue 下评论建议拆分

**Risk:**
- 注入格式变化可能影响既有用户 prompt 的行为回归(token 占用略增)——靠 PR-1 的测试用例兜住
- workspace 身份定义有歧义(以 cwd?以项目根?)——实现前宜在 issue 下与维护者确认口径
- state 旧格式兼容:已有用户的 `hook-state/*.json` 需迁移或容错读取

**Estimated scope:** ~150-250 LOC(边界包裹 + 测试 ≈80;workspace 隔离 ≈80;摘要降级另计)

**Verdict: 值得认领** — 三条理由:①reporter 给出了精确行号级定位与复现步骤,issue 质量极高,修复面收敛;②主诉恰是 Context/Memory 注入安全,与认领者的日常研究方向(会话检索/上下文工程)完全同域;③先提 PR-1 这种小而准的修复是建立维护者信任的最短路径。**行动建议:先在 issue 下评论确认 workspace 口径与拆分意向,再动手 PR-1。**
