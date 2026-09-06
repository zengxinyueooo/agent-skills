---
name: agent-session-handoff
description: 把进行中的 Coding Agent 会话(Claude Code / Codex / ThoughtCoding 等)压缩成一份可迁移的 handoff 文档——换 Agent、换机器、隔天继续时不丢状态。Use when the user says "做一次 handoff" / provides a session file or transcript to compress. Do not use for executing next steps, judging code quality, or rewriting the user's commit messages.
---

# Agent Session Handoff

把一个正在进行的 coding session 压缩成下一个人(或下一个 Agent)能直接接手的 artifact。

出身:来自我对 AI Agent 会话持久化的研究([AgentRecall](https://github.com/zszz3/AgentRecall) 相关工作)与自研 [ThoughtCoding](https://github.com/zengxinyueooo/ThoughtCoding) 的四层上下文压缩管线——前者是"事后找回来",这个 skill 是"主动交出去"。

## 为什么需要

Context 压缩、跨 Agent 迁移、第二天继续,都会丢三类东西:为什么这么做(决策理由)、试过什么没成(失败尝试)、现在停在哪(精确状态)。通用摘要会美化这些,handoff 文档把它们当作一等公民。

## Start Gate

满足其一:

- 用户给出 session 数据来源:宿主的会话文件路径(JSONL / sqlite / 导出文本)或直接粘贴的对话记录;
- 用户在当前对话中直接说"做一次 handoff"(此时当前对话即源会话)。

来源缺失 → 停,问一句要哪种。

## Workflow

1. **识别宿主**:确认 session 来自哪个宿主(Claude Code / Codex / ThoughtCoding / 其他),决定提取器;未知格式时先看文件结构,别猜;
2. **提取信号**,只采集这五类,每类必须能在源数据中指出位置:
   - 用户表达过的目标与约束(原话关键词,不是转述);
   - 已完成:落盘的改动、通过的验证;
   - 失败尝试:试了什么、报什么错、为什么放弃——**原样保留,不美化**;
   - 关键文件:改动过 / 依赖过的路径;
   - 当前状态:最后一个动作、待验证项;
3. **生成 handoff 文档**(schema 见下);
4. **Grounded 校验**:文档里出现的每个文件路径必须真实存在(能 ls 到),每条结论必须能对应源 session 中的一段记录,对应不上的删掉或标 `unknown`。

## Output Contract

固定七节,文件名 `handoff-<YYYYMMDD-HHmm>.md`:

```markdown
# Session Handoff
> 源:<宿主> · <session 文件/时间范围> · 生成于 <时间>

## Current Goal
<用户自己的目标表述,允许引用原话>

## Completed
- <改动> — 验证:<怎么确认的>

## Failed Attempts
- <尝试> — 失败原因:<报错/现象> — 处置:<放弃原因或遗留线索>

## Important Files
- <路径> — <为什么重要>

## Constraints
- <用户给过的限制:技术选型、风格、禁改区、截止时间>

## Current State
<此刻精确状态:什么在跑、什么没验证、工作区是否干净>

## Next Steps
1. <下一步,按优先级,可直接执行的粒度>
```

## Boundaries

- 只记录,不执行 Next Steps;
- 失败尝试不省略、不归因猜测——失败原因写观察到的现象,不写"可能是";
- 不确定的就写 `unknown`,一份诚实但薄 的 handoff 远比一份流畅但有编造的 handoff 有用;
- 产物是纯 markdown,放哪里由用户指定;默认与项目同级,不进 `.git`(除非用户要求)。

## 下游

- 直接喂给任何 Agent 的首条消息:"先读 handoff 文档再动手";
- 与 AgentRecall 类工具的迁移输入兼容(七节 schema 即索引字段);
- 求职场景:session handoff 是 star-bank Action 条目的证据源之一。
