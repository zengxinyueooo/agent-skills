---
name: reference-note-capture
description: 对一条人工保留(kept)的参考内容,采集完整元数据与媒体清单形成持久研究资产,并把有效媒体交给 asset-ingestion。Use only after the user keeps an inbox note and explicitly starts capture. Do not use for discovery batches, topic synthesis, Brief generation, or publishing.
---

# Reference Note Capture · 笔记完整采集

把一条人工保留的参考内容固化为持久研究资产。「保留」这个决定已经由人做出;**本 skill 不做第二次保留判断**。

## Start Gate(全部满足)

- 用户对 inbox 中一条笔记显式点击/指令「采集完整信息」;
- 该笔记属于当前工作区活动领域,且有来源 URL 或稳定 note ID;
- 该笔记 `status: kept`。

未保留的 `list_only` 候选 → 停,退回人工审核。**绝不采集检索结果批次。**

## Capture Contract

1. 按 DOMAIN.md 配置的适配器(使用已有登录态)执行采集;先读适配器使用说明;
2. 连通性自检一次;取一次所选内容详情,**遇验证页 / 过期签名 URL / 拒绝访问 / 异常提示即停**,不反复重试;
3. 只持久化详情页返回的事实:稳定 note ID、规范来源 URL、标题、正文、话题标签、作者、发布时间、可见互动数、媒体数量与顺序、检索时间、`detail_status: captured`;
4. 保留列表行的查询词与指标作为检索溯源——详情值**不覆盖**列表历史;
5. 对有效图片调用 `asset-ingestion`,其默认产出是 `pending` 待人工审核,不是自动生成的语义标签。

## Output Contract

落盘 `captures/<note-id>.md`:上述全部事实字段 + 媒体清单(序号/文件名/类型),`detail_status: captured`。展示为「已采集,待素材审核」。

## Stop Gate

元数据与媒体持久化后即停,**不做**:判断单图/拼图、为 Brief 选素材、创建选题或 Brief、生成文案、发布。

## Safety and Traceability

- 平台动作只读:不点赞、不保存、不关注、不评论、不发布;
- 绝不把来源文案复制进生成物——正文与标签是带溯源的研究证据;
- 保留来源 note ID 与图片顺序供去重;
- 不暴露登录凭据与存储密钥。
