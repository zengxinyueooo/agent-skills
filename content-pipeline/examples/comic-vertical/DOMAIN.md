# DOMAIN · 漫画二创内容垂直(示例领域包)

这是一个**示例**领域包,展示 DOMAIN.md 该写什么。换领域 = 复制本文件改内容,核心技能零改动。

```yaml
domain: 漫画二创图文内容
platform: 小红书(图文笔记)
adapter: opencli-xiaohongshu        # 见 content-pipeline/adapters/

# —— 信源(niche-discovery 的官方证据来源)——
source:
  name: 快看漫画
  official_verify: 快看 App / 官方站内作品页(可直接打开的作品详情 = 官方证据)
  fields: [规范名, 官方URL, 作者, 连载状态, 更新星期(仅当官方标明), 核实日期]

# —— 查询策略(platform-reference-discovery)——
query_strategy:
  ongoing:    # 时效型:围绕最新章节/更新点,短时窗
    intents: ["<作品名> 更新", "<作品名> 新章", "<作品名> 名场面"]
    window_days: 7
  completed:  # 长尾型:耐久角度,不查最新、不设时窗
    intents: ["<作品名> 名场面", "<作品名> 重温", "<作品名> 意难平"]
    window_days: null
  rules:
    - 作品名必须置首位,意图词在后;禁止裸搜意图词
    - 每作品 ≥3 个不同意图;白月光/意难平/角色名等仅当测试不同意图时使用
    - 只读首屏;全部查询合计 ≤10 条;按 note ID 去重

# —— 素材规则(asset-ingestion / brief-generation)——
asset_rules:
  brief_eligible: "仅 continuous 单图;拼图 rejected 但保留在库"
  copyright: reference_only
  max_per_brief: 3

# —— 标签体系(仅当视觉分类实际发生时使用,≤3 个/图)——
tags:
  emotion: [甜, 暧昧, 心动, 治愈, 轻松, 紧张, 虐心, 悬念, 反差萌]
  story_beat: [对视, 承诺, 守护, 吃醋, 信任危机, 关系推进, 设定揭秘]
  visual: [双人同框, 双人对话, 人物特写, 亲密距离, 萌宠]

# —— 人工节点文案 ——
review_labels:
  pending_brief: 待你审核
  pending_asset: 已采集,待素材审核
```

## 评分参考(niche-discovery 第 5 步)

见 [references/scoring-and-output.md](references/scoring-and-output.md)。
