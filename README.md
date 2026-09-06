# agent-skills

一套从真实项目和真实工作流里沉淀出来的 Agent Skills。

我的原则:**不为了数量做 skill**。每个 skill 必须满足——我自己真实用过、grounded(读真实文件 / 调真实 API,不是纯 prompt 总结)、有明确的触发条件与停止边界、能和一个主项目互相印证。

## 三层版图

skill 不该是一堆孤岛。下面三层互相有数据流动:session → star-bank,面经复盘 → star-bank,star-bank → 简历与面试准备。

### 求职流水线(自用,直接服务秋招)

| Skill | 一句话 | 依赖 |
|---|---|---|
| [star-bank](star-bank/) | STAR 经历故事库——一次性投入、反复取用的资产,所有下游 skill 的数据源 | — |
| [resume-tailor](resume-tailor/) | 粘贴 JD → 定制简历 bullets + 匹配度报告 + 诚实补足建议 | star-bank |
| [interview-coach](interview-coach/) | 终端里当面试官:预测题、模拟追问、点评,错题自动归档 | star-bank |

### 展示层(featured,从主项目沉淀)

| Skill | 一句话 | 出身 |
|---|---|---|
| [agent-session-handoff](agent-session-handoff/) | 把 Coding Agent 会话压缩成可迁移的 handoff 文档 | AgentRecall 研究 / ThoughtCoding 上下文压缩 |
| content-pipeline | 带人工门禁的内容生产 SOP(7 个子技能 + 可插拔领域包) | Creator Ops Studio(移植中) |
| companion-memory-eval | 陪伴型 AI 的记忆质量评测(事实/情节/人设/时效/过度召回) | Love & Secret(规划中) |
| repo-onboarding | 仓库 → 带来源标注的结构化 onboarding 报告 | 日常读开源(规划中) |
| issue-pr-triage | Issue → 难度定位 + 实现计划 + 查重 | multica 开源参与(规划中) |
| xhs-digest | 收藏夹消化器:抓取提炼去重,收敛成 checklist(utility) | 个人知识管理(规划中) |

### 底座

star-bank 严格说不是 skill 而是资产——但它有维护协议,所以也按 skill 的形态放在这里。真实内容永远存在仓库之外(本地笔记/Obsidian),仓库里只有协议和模板。

## Skill 编写约定

所有 skill 遵循同一骨架,这套约定来自我在 [ThoughtCoding](https://github.com/zengxinyueooo/ThoughtCoding)(开源 Coding Agent CLI,84+ Stars)里实现技能系统的经验,以及内容工作台里「人工审核门」的实践:

- **frontmatter**:`name` + `description`(含触发时机与不应使用的场景)
- **Start Gate**:什么条件齐了才允许开始,缺一项就停
- **Workflow**:编号步骤,每步有明确产物
- **Output Contract**:输出 schema 固定,机器可解析
- **Stop Gate / Boundaries**:到哪一步必须停下来等人;哪些事坚决不做
- **Grounded**:结论必须标注来源文件路径 / 证据 ID;不确定标 `unknown`,不编

## 目录约定

每个 skill 一个目录,`SKILL.md` 为入口,可附 `templates/`(模板)、`references/`(评分标准等)、`examples/`(真实产物示例,脱敏)。

## License

[MIT](LICENSE)
