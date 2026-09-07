---
name: brief-generation
description: 从一个人工确认的选题及其挂接的已采集参考,生成一份结构化、可编辑的内容 Brief 供人审核。Use only after the user explicitly triggers Brief generation. Do not use for research, asset selection, Brief approval, draft generation, or publishing.
---

# Brief Generation · 内容 Brief 生成

生成一份**供人审核的提案**,不是执行指令。

## Start Gate(全部满足)

- 选题存在于 `topics/` 且 `status: confirmed`;
- 选题至少挂接一条 kept + captured 的同领域参考;
- 用户显式触发「生成 Brief」或「重新生成 Brief」。

## Output Contract

落盘 `briefs/<topic-slug>/v<N>.md`,`status: candidate`,内容恰好包含:

- 内容角度与一个非衍生化的开头钩子;
- 核心情绪关键词(从 DOMAIN.md 标签体系取);
- 三步式内容结构;
- 三条配图需求——**表述为视觉需求,不是"用某张来源图"的指令**;
- 剧透与原创性护栏(按领域特性定义);
- 挂接的证据/参考 ID;若使用了模型,记录模型/提供商元数据。

配置了文本模型则用之;无模型时输出确定性模板兜底并标注 `generation_mode: template`,**绝不冒充模型结果**。

不复制参考文案。不发明证据中不存在的领域事实。

## Human Gate and Stop Condition

Brief 以「待你审核」呈现。用户可批准 / 拒绝 / 重生成。**批准前停止**——素材选择、草稿生成、发布都不做。approved 的 Brief 是素材评审阶段的唯一入场条件(批准 = 用户改 `status: approved`)。
