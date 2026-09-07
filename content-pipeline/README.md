# content-pipeline

一套**带人工门禁的内容生产 SOP**:从细分领域验证 → 平台参考调研 → 素材采集 → 选题 → Brief → 文案草稿,共 7 个子技能,每个对应状态机的一个状态,所有状态转换必须由人确认。

设计原则:skill 负责生产"待审核的候选物",**人负责所有放行决策**。平台操作全程只读;发布永远是人的手动动作。

## 状态机总览

```
niche-discovery
    └─→ sources/  status: candidate ──[人工:选定领域]──→ selected
platform-reference-discovery (对 selected 领域)
    └─→ inbox/  status: candidate (detail_status: list_only) ──[人工:保留/丢弃]──→ kept | rejected
reference-note-capture (对 kept 笔记,一次一条)
    └─→ captures/  detail_status: captured
asset-ingestion (随采集执行)
    └─→ assets/<note-id>/  review_status: pending ──[人工:审核]──→ available | rejected(保留不删)
topic-synthesis (对同领域 1-4 条 kept+captured 参考)
    └─→ topics/  status: inspiration ──[人工:确认选题]──→ confirmed
brief-generation (对 confirmed 选题,显式触发)
    └─→ briefs/  status: candidate ──[人工:批准]──→ approved
draft-generation (对 approved Brief + 人工选定素材)
    └─→ drafts/  status: draft ──[人工:编辑并手动发布]──→ published
```

## 文件级状态约定(无需数据库)

所有状态存放在使用者声明的工作区目录(默认 `./pipeline-workspace/`),状态写在 markdown 文件的 frontmatter:

```
pipeline-workspace/
├── sources/<niche-slug>.md          # status: candidate | selected | rejected
├── inbox/<note-id>.md               # status: candidate | kept | rejected (list_only)
├── captures/<note-id>.md            # detail_status: captured + 媒体清单 + 来源溯源
├── assets/<note-id>/                # 图片文件 + manifest.md (review_status: pending | available | rejected)
├── topics/<topic-slug>.md           # status: inspiration | confirmed
├── briefs/<topic-slug>/v<N>.md      # status: candidate | approved | rejected
└── drafts/<topic-slug>/v<N>.md      # status: draft | published
```

规则:
- **skill 永远不自己推进状态**——下一个状态由使用者改 frontmatter 或显式确认指令触发;
- 去重主键 = 平台稳定 note ID(+ 素材加图片序号),不用会过期的签名 URL;
- 每条记录保留溯源:来源 URL、note ID、命中的查询词、检索时间。

## 领域包(可插拔)

核心技能不含任何特定平台/领域逻辑。领域相关的一切写在 `DOMAIN.md`(放在工作区根目录),skill 开工前先读它:

- 平台与适配器(见 [adapters/](adapters/));
- 信源与查询策略(时效型 vs 长尾型选题);
- 素材资格条件与标签体系;
- 人工审核节点文案。

示例领域包:[examples/comic-vertical/DOMAIN.md](examples/comic-vertical/DOMAIN.md)(漫画二创内容垂直)。换领域 = 写一个新的 DOMAIN.md,核心技能零改动。

## 七个子技能

| # | Skill | 输入 → 输出 | 停在哪 |
|---|---|---|---|
| 1 | [niche-discovery](niche-discovery/) | 账号定位 → 领域候选清单(candidate) | 选域由人 |
| 2 | [platform-reference-discovery](platform-reference-discovery/) | selected 领域 → 参考候选(list_only) | 导入由人 |
| 3 | [reference-note-capture](reference-note-capture/) | kept 笔记 → 完整元数据+媒体清单(captured) | 素材审核由人 |
| 4 | [asset-ingestion](asset-ingestion/) | 采集的媒体 → 素材库记录(pending) | 审核由人 |
| 5 | [topic-synthesis](topic-synthesis/) | 同域参考 1-4 条 → 选题候选(inspiration) | 确认由人 |
| 6 | [brief-generation](brief-generation/) | 确认选题 → 结构化 Brief(candidate) | 批准由人 |
| 7 | [draft-generation](draft-generation/) | approved Brief + 选定素材 → 文案草稿(draft) | 发布由人 |

## 安全红线(全管线通用)

- 平台操作**只读**:不点赞、不收藏、不关注、不评论、不自动发布;
- 遇验证页 / 异常弹窗 / 拒绝访问 → 立即停止,**不自动重试**;
- 不爬取、不绕过登录墙,限速自然(小结果集、首屏即止);
- 不复制他人文案与图片用作原创:参考素材 `copyrightStatus: reference_only`,生成物必须原创;
- 无模型可用时输出确定性模板兜底,并明确标注 `generation_mode: template`,**不冒充 AI 生成**;
- 不在产物中暴露任何登录凭据或存储密钥。
