---
name: asset-ingestion
description: 把一条已采集笔记的全部有效图片存入工作区素材库,带来源溯源,默认 pending 待人工审核。Use only after reference-note-capture has persisted the note. Do not use for broad research, automatic image judgment, topic creation, Brief generation, or publishing.
---

# Asset Ingestion · 素材入库

把一条已审核笔记转化为可复用、可溯源的参考素材。入库产物是**素材库记录**,不是可用性声明,更不是版权声明。

## Required Scope

本 skill 是 `reference-note-capture` 的存储环节:仅在用户对 kept 笔记显式发起采集、详情元数据已落盘、目标领域已知时运行。确认输入:

- 已采集笔记的 note ID 与媒体清单(`captures/<note-id>.md`);
- 活动领域与 DOMAIN.md 中的素材规则;
- 是否全量保留:默认保留所选笔记的全部有效图片。

不扩大到创作者主页、检索批次或另一条笔记。入库过程不创建选题/Brief、不发布。

## Collect Safely

- 经 DOMAIN.md 配置的适配器下载;先做连通性自检一次;
- 用已采集的媒体清单下载,**不重新拉取详情**;
- 一次性下载到 `assets/<note-id>/`(临时目录若适配器要求,入库后清理);
- 保留 note ID、标题、作者、来源 URL、话题标签、图片顺序——签名 URL 会过期,**note ID 是持久去重主键**;
- 只读原则:不点赞、不保存、不关注、不评论、不重复刷新。

## Store First; Review Separately

只做「能否作为图片文件存储」的检查(损坏/非图片 → 剔除并说明),**不改变尺寸、不再压缩**。

未配置视觉模型时,不声称图片已被分类。每张入库资产默认:

```yaml
visual_format: uncertain
review_status: pending
content_type: other
classification_note: 已从参考笔记采集,待人工确认图型与内容标签(第 N 张)
tags: []            # 无语义标签,不猜人物/内容
```

若 DOMAIN.md 允许且用户显式请求了视觉预分类,其结果**仍然可审核**,只允许预填最小集合(visual_format / review_status / classification_note / 明显的 content_type),人物名只在确凿时添加,否则不猜。

人工在素材审核时把单张图片标记为 `available`(或按 DOMAIN.md 规则 rejected——被拒素材**保留在库**,只是不可被 Brief 引用)。

## Write Contract

`assets/<note-id>/manifest.md` 每张图片必须包含:

- 原始文件名、MIME 类型、字节大小、存储路径;
- `source_type` + 原始来源 URL + 稳定 `sourceNoteId`;
- `copyrightStatus: reference_only`(除非用户提供更强权利信息);
- 所属领域、笔记标题派生的章节/段落标签、图片序号;
- 语义标签(仅当实际发生分类):每图 ≤3 个,从 DOMAIN.md 标签体系选;笔记话题标签只作候选证据——去重后存入 `sourceNoteTags` 留痕,仅当其真实描述这张图时才提升为可见标签。不用占位标签(如「测试」)。

去重:活动领域内按 note ID + 图片序号/原文件名,已存在则保留原资产不重复入库。**绝不自动把素材挂到 Brief**——那是人工审核动作。

## Verify the Result

入库后核验:入库数量 = 有效下载数(含去重说明);每条记录可预览且能打开原图;报告总数、pending 数、rejected 数(如有人工/模型确认)、被跳过文件及原因。

## Boundaries

- 来源图片仅作参考,不冒充原创,不绕过人工发布审核;
- 不存储损坏/非图片/超出所选笔记媒体清单的文件,说明原因;
- 不暴露存储凭据与登录凭据;
- 仅依据重复出现的工作流证据改进本 skill,范围限于单笔记素材入库。
