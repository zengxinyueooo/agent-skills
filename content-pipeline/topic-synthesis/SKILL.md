---
name: topic-synthesis
description: 把同领域的 1-4 条人工保留且已完整采集的参考,合成一个可编辑的选题候选并挂接证据。Use after reference capture and before Brief generation. Do not use for searching, media ingestion, Brief approval, draft generation, or publishing.
---

# Topic Synthesis · 选题合成

从用户**刻意保留**的证据合成可编辑选题。选题是内容假设,不是获批的 Brief。

## Start Gate

用户已选定 1-4 条参考,且全部满足:

- 同属一个领域;
- `status: kept`;
- `detail_status: captured`。

1 条参考仅在明确支撑一个独立角度时允许;优先 2-4 条以获得更强证据。不混用不同领域的参考,不用 list-only 候选充当证据。

## Create the Candidate

只产出并持久化以下字段(落盘 `topics/<topic-slug>.md`,`status: inspiration`):

- 简洁选题标题;
- 领域与可选的范围限定(章节/场景/子话题);
- 内容角度与期望读者反应;
- 挂接的参考 ID 及各自的证据要点;
- 角度成立的原因(互动信号 / 反复出现的讨论点 / 更新相关性);
- 证据薄弱时明确写不确定。

措辞基于底层场景与跨内容共性,**不复述来源的标题、文案或标志性句子**。

## Human Gate and Stop Condition

用户在选题看板(或直接编辑文件)审阅并可修改候选。保存后即停,**不做**:

- 自动创建 Brief;
- 把选题标记为已确认/已批准(确认 = 用户改 `status: confirmed`);
- 选素材或挂接素材;
- 生成文案或发布。

下一个显式动作是「生成 Brief」,由 `brief-generation` 处理。
