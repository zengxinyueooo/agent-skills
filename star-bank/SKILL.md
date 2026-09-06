---
name: star-bank
description: 维护个人 STAR 经历故事库——简历 bullet、面试答案、复盘的统一数据源。Use when recording a new experience, updating an existing story after an interview, or retrieving material for resume-tailor / interview-coach. Do not use for generating resumes or conducting interviews (downstream skills do that).
---

# Star Bank · 经历故事库

这是资产,不是生成器。简历、面试、复盘都从这里取材;没有它,下游 skill 全是空转。

## 数据边界

真实经历数据**永远不存在本仓库**,存在你自己的笔记系统里(默认 `~/notes/star-bank/` 或 Obsidian vault,路径可在会话开头声明)。本 skill 只定义协议和模板。

## 目录约定

```
star-bank-local/
├── index.md              # 索引:一段一行,id / tags / 一句话 / 证据数
└── experiences/
    ├── 2025-10-meituan-agent-pipeline.md
    ├── 2026-03-thoughtcoding-skill-system.md
    └── ...
```

## Start Gate

开始写入前确认:

- 有真实证据源:一段 coding session 的 handoff、git commit 历史、实习周报、项目上线记录,至少其一;
- 用户本人参与过(面试口头素材需用户亲述,不代写);
- 该经历与库中已有条目不重复(先查 `index.md`)。

缺证据源的条目标 `evidence: missing`,允许先写骨架,但简历引用前必须补齐。

## 写入协议

每条经历一个文件,遵循 [templates/experience-template.md](templates/experience-template.md):

1. **Situation / Task**:背景与任务,3 句话内,含规模数字(账号数、QPS、代码量、人数);
2. **Action**:你个人做了什么(不是"团队做了什么"),每条 action 标注对应证据(文件路径 / PR 链接 / 周报日期);
3. **Result**:量化结果 + 一个诚实的"未解决/遗憾"字段——面试深挖时的诚实底线;
4. **三个可复用版本**:简历 bullet 版(≤2 行) / 一分钟口述版 / 深挖追问预备版(预判 3 个追问及答法)。

## 更新协议(飞轮所在)

- 每次面试后:被问倒的问题 → 检查是哪条经历的故事没准备好 → 回填对应文件的「深挖追问预备版」;
- 每次项目节点后:从 git log / session handoff 提取增量,追加 Action 条目;
- 每月一次:跑通 `index.md` 与文件的字段一致性,孤儿文件归位。

## Boundaries

- 不编造:没有证据支撑的数字一律标 `待核实`,禁止进入简历;
- 不删除:过时经历标记 `archived: true`,不删文件(校招季任何经历都可能被翻出来);
- 不代写:skill 负责结构和追问,经历本身必须是用户的真实记忆。
