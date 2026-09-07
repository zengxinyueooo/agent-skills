---
name: draft-generation
description: 从一份已批准的 Brief 和人工选定的合格素材,生成可编辑的标题与正文草稿。Use only after Brief approval and material selection. Do not use for Brief approval, asset review, automated publication, or account actions.
---

# Draft Generation · 文案草稿生成

把获批的计划与选定素材变成**可编辑的草稿**。最终编辑与手动发布永远由人负责。

## Start Gate(全部满足)

- 一份 Brief `status: approved`;
- 用户已选定至少一条挂接到该 Brief 的素材;
- 每条素材满足 DOMAIN.md 的资格条件(默认:状态 `available`,且符合领域包对图型/数量的限制);
- 用户显式触发「基于 Brief 生成」或「生成新版本」。

## Output Contract

落盘 `drafts/<topic-slug>/v<N>.md`,`status: draft`,含标题、正文、可选话题标签,并回链 Brief ID 与所用素材 ID。草稿必须:

- 遵循 Brief 的角度、结构与护栏约束;
- 不提及证据不支持的领域事实;
- 原创表达,不是对来源笔记的改写;
- 保留生成模式/模型元数据;
- 不改动任何来源素材的使用计数(计数只在人工标记"已发布"时更新)。

无文本模型时生成明确标注的模板兜底,不声称是 LLM 生成。

## Stop Gate

保存为 `draft` 并呈现给用户编辑后即停,**不做任何平台交互**。只有单独的、人工的「已发布」动作能把已发布的草稿标记为 published,并使所选素材的使用计数 +1。
