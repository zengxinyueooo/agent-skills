---
name: companion-memory-eval
description: 评测陪伴型 AI 的记忆质量——事实召回、情节召回、人设一致性、时效更新、过度召回五个维度,输出分维度报告与错因分类。Use when the user provides a conversation dataset and a memory-backed companion system to evaluate. Do not use for academic benchmark reproduction, evaluating non-memory chatbot quality, or generating golden datasets without human confirmation.
---

# Companion Memory Eval

不是"为了做评测而评测":陪伴型 AI 的核心资产是记忆,但"它记得我吗"从来没人量过。这个 skill 把它变成一份可复跑的报告。

## Start Gate

同时满足才开工:

1. 对话数据集:真实对话导出(文件 / 数据库导出),≥30 轮,含时间戳;
2. 被测记忆系统可调用:能以"注入历史 → 提问 → 取回答"的方式运行至少一个配置;
3. 用户确认:评测维度、题目规模、评分严格度。

三者缺一 → 停。金标准没经人工确认 → 停。

## Workflow

1. **构建金标准集**:从真实对话抽取 QA 对(30-50 条),每条标注维度——
   - `fact`:用户说过的确定事实(生日、名字、偏好);
   - `episode`:共同经历过的情节("我们上次聊到 X 时我说了什么");
   - `persona`:人设一致性(语气、称呼、设定不漂移);
   - `temporal`:信息更新("我换了工作"之后旧答案应失效);
   - `over-recall`:**不该**记起的信息被主动提起(负样本,答"不记得/不知道"才算对);
2. **运行被测系统**:逐题提问,记录回答 + 系统引用的记忆原文;
3. **评分**:确定性断言(事实/数字/日期精确匹配)+ LLM-as-judge(开放题,judge 提示词固定,1-5 分 + 理由);judge 模型不得与被测系统同模型;
4. **错因分类**(本 skill 的核心产物):每道错题归入——`没写入`(存储问题)/ `存了没召回`(检索问题)/ `召回了答错`(生成问题)/ `幻觉`(无中生有);
5. **基线对比**(推荐):同一题集跑两遍——原始历史全文注入 vs 摘要记忆注入,差异即"压缩的代价/收益"。

## Output Contract

`memory-eval-report-<YYYYMMDD>.md`:

```
## 总分
| 维度 | 题数 | 通过率 | 平均分 |
## 错因分布
| 错因 | 数量 | 占比 | 典型样本(题-期望-实际) |
## 分维度明细
<逐题:题目 / 期望 / 实际 / 判定 / 错因 / 引用的记忆原文>
## 建议
<按错因分布给改进方向:写入侧 / 检索侧 / 生成侧>
```

## Boundaries

- 金标准必须人工逐条确认,不自动生成后自评自;
- 样本量小,报告只代表方向性结论,措辞禁止"证明";
- 对话数据先脱敏再入库,报告公开前二次检查;
- over-recall 是陪伴场景的灵魂维度,不允许为了总分好看删掉负样本。
