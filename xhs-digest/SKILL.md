---
name: xhs-digest
description: 个人收藏夹消化器——把小红书链接/截图/复制的文案提炼成可执行要点,打标签、去重合并进个人知识库,重复观点自动收敛为 checklist,每周生成 digest。Use when the user forwards a note link, screenshot, or pasted copy for archiving, or asks for the weekly digest. Do not use for bulk crawling, re-publishing others' content, or bypassing login walls and captchas.
---

# XHS Digest · 收藏夹消化器

解决的问题:收藏 ≠ 学会。"简历三不要"这类内容收藏了 20 遍,价值在于把 20 篇收敛成 1 条进 checklist。

出身:个人知识管理工作流。抓取基于通用网页读取能力(**built on top of** 公开的网页读取工具),本 skill 的增量是结构化提炼 + 去重合并 + 周报。

## 定位声明

Utility skill,不是 featured 作品。同类抓取工具很多,它的差异只在"消化"这半段。平台抓取属灰色地带:仅限个人存档,不可宣传为通用爬虫。

## Start Gate

- 输入:笔记链接 / 截图 / 粘贴的文案,至少其一;
- 知识库路径已声明(默认用户的 Obsidian vault 子目录);
- 周报模式:直接说"生成周报",无输入前置。

## Workflow — 单条入库

1. **采集**:链接 → 读取正文与图片清单;截图 → 视觉提取文字;粘贴 → 直接使用。元信息:标题 / 作者 / 链接 / 发布日期 / 采集时间;
2. **提炼**:只提**可执行要点**(能变成动作的话),每条 ≤2 行,注明出处小节;禁止只存"情绪价值金句";
3. **打标签**:从知识库已有标签表选,没有就新建并报告;
4. **去重合并**:与库内条目比对,观点高相似 → 不新建,合并进已有条目的「多源印证」区,`dup_count +1`;某观点 `dup_count ≥ 3` → 自动提炼一行进 `checklist.md` 对应分区;
5. **回执**:一行报告(新增 / 合并进哪条 / 是否触发 checklist 升级)。

## Workflow — 周报

每周一运行:上周新增条目清单 / 新升级的 checklist 项 / **与已有观点冲突的内容**(如两篇对"要不要写自我评价"结论相反,并列呈现,由用户裁决)/ 标签统计。

## Output Contract

单条文件 frontmatter 固定:

```yaml
source: <URL / 截图 / 粘贴>
author: <作者>
captured: <日期>
tags: [...]
dup_count: <n>
```

`checklist.md` 分区固定:求职 / 简历 / 面试 / 运营 / 其他。

## Boundaries

- 仅个人存档与学习:不批量爬取、不复述超出摘要必要范围的原文、不公开转贴他人内容;
- 遇登录墙、验证码、异常弹窗 → 停,不重试不绕过;
- 提炼必须转译为用户自己的话,原文只留出处;
- checklist 只进"可执行 + 已多源印证"的观点,单一来源的观点留在条目里等印证。
