---
name: platform-reference-discovery
description: 为一个已选定的细分领域,在目标平台检索并产出一小批去重后的图文参考候选,交由人工显式导入。Use after a niche is selected and before full-note capture. Do not use for importing, reading full note details, media download, topic creation, Brief generation, copywriting, or publishing.
---

# Platform Reference Discovery · 参考候选发现

为一个已选定领域产出**可审核**的参考结果集。本 skill 止于用户的显式导入决策。

## Boundaries(先读)

- 每次任务只针对**一个**选定领域;
- 检索结果只是参考候选,不是选题;
- 浏览操作只读;遇验证、异常弹窗、访问拒绝即停,不自动重试;
- 不打开内容详情页、不下载媒体、不创建 capture 记录;
- 不发明详情:`list_only` 结果只允许 title / author / 互动数 / 日期 / URL / note ID 六类字段;
- 绝不创建选题或 Brief。

## Workflow

1. 确认领域已存在于 `sources/` 且 `status: selected`;
2. 按 DOMAIN.md 的查询策略选词与时窗:时效型对象围绕最新更新点组 2-3 个含名称的查询、用短时窗;长尾型对象用耐久角度(经典场景 / 人物互动 / 常见讨论点),不查"最新"、不设时窗;
3. 经适配器(只读,使用已有登录态)**只读首屏**;本地按互动排序;时效型对象只保留时窗内内容。「未见过」= 未导入过本工作区,不是平台私有浏览标记;
4. 按 note ID 去重(缺失时回退签名 URL),全部查询合计**最多保留 10 条**;
5. 展示结果,**只导入用户显式勾选的行**。

## Output Contract

每条导入记录落盘 `inbox/<note-id>.md`,`status: candidate`、`detail_status: list_only`,frontmatter:来源 URL、note ID、命中查询词、列表标题、作者、互动数、发布日期、领域关联、检索时间。不写其他任何字段。

## Stop Gate

导入 ≠ 保留。候选入库后停在 research inbox,**保留 / 丢弃是下一个人工动作**;完整采集必须由用户对 kept 笔记显式发起,交由 `reference-note-capture`。
