---
name: niche-discovery
description: 验证并筛选内容细分领域候选——核实官方信源,用目标平台的公开热度与时效信号评估需求,产出证据支撑的候选清单。Use when building or refreshing the account's niche candidate library. Do not use for topic or material research on an already selected niche, and never auto-select a niche.
---

# Niche Discovery · 细分领域验证

为账号定位产出**证据支撑**的领域候选清单。一切结果都是 candidate,选定永远由人。

## Start Gate

- 已读工作区根目录的 `DOMAIN.md`(信源、平台适配器、账号定位);
- 明确目标:新建候选库 or 补充已有库;
- 可选输入(缺失不阻塞,声明合理假设后继续):账号定位简述、期望清单规模(默认 10)、用户已读/喜欢/明确拒绝的对象。

## Workflow

1. **种子池**:从 DOMAIN.md 指定的官方信源收集候选对象(官方榜单/推荐/检索结果/用户补充),记录发现来源与日期。**一般网络热度不能作为该平台可用的证明**;
2. **信源硬门**:每个候选必须找到 DOMAIN.md 认可的官方直接证据页,记录:规范名、官方 URL、作者(如有)、连载/更新状态(`ongoing` / `completed` / `unknown`)、明确的更新日(否则 `null`,**不推断**)、核实日期;官方可用性无法核实 → 拒绝该候选;
3. **平台需求验证**:经 DOMAIN.md 配置的适配器(只读)检索。每次查询必须含规范名(或已登记别名)且置于首位,意图词在后;每个候选至少 3 个不同意图的查询;时效型对象补更新导向查询。保留:查询词、检索时间、内容 URL/ID、标题、作者、原始互动数、发布时间——**缺失数据标 unknown,不编造**;
4. **相关性校验**:标题/正文/话题标签均不含规范名或别名的结果,判为误召回剔除并保留剔除理由,不静默计入需求证据;
5. **证据评估**:读 `references/scoring-and-output.md`(若领域包提供)后打分,分离四类信号——已证明需求(相关内容互动量)/ 当前需求(近期相关活跃)/ 可复用广度(不同讨论角度数)/ 账号契合度。单一异常值不构成"可能爆"的结论;
6. **产出候选表**:按 schema 输出排序表 + 结构化记录,含理由、证据 URL、不确定性、被拒对象及原因;与已有记录按规范名+别名去重,不静默覆盖用户填写字段。

## Output Contract

落盘 `sources/<niche-slug>.md`,`status: candidate`,frontmatter 含:规范名、官方信源 URL、状态、核实日期、证据摘要(查询词+样本内容 ID+互动概况)、四类信号评分、被拒邻居及原因。

## Stop Gate

写入后即停。**绝不**:替用户选定领域、把 candidate 标为 selected、直接进入参考调研。下一个动作是用户改 `status: selected`。

## Boundaries

- 平台操作只读;不下载、不复制他人图片;不复制文案(标题短语仅作研究证据保留);
- 限速自然:聚焦查询、小结果集、不穷举;
- 每个入选候选都要有官方信源 + 平台内容双重直接证据;
- 本 skill 只做领域发现,已选领域的选题与素材调研交给下游技能。
