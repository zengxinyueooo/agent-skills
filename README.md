# agent-skills

一组可直接装入 Coding Agent 的开源 Skills:求职流水线、代码仓库阅读、开源贡献、Agent 会话交接、记忆评测、内容生产 SOP。

所有 skill 遵循同一套编写约定:**Start Gate(什么条件才开工)→ Workflow(固定步骤)→ Output Contract(机器可解析的输出)→ Stop Gate / Boundaries(到哪必须停、哪些事不做)**。结论必须 grounded——读真实文件、调真实 API,输出标注来源,不确定就写 `unknown`。

## Quick Start

把这些 skill 装进你正在用的 agent:

| 宿主 | 安装方式 |
|---|---|
| Claude Code | `git clone` 本仓库,把想要的 skill 目录拷到 `~/.claude/skills/`(或项目级 `.claude/skills/`) |
| ThoughtCoding 等 skill 机制 CLI | 拷贝 skill 目录到其 `skills/` 目录(标准 `SKILL.md` + frontmatter 格式) |
| 其他 Agent(Cursor / 通用) | 把对应 `SKILL.md` 全文作为项目规则 / 系统提示词的一部分,目录里的 `templates/`、`references/` 一起带走 |

验证安装:对 agent 说「列出你当前可用的 skills」或直接触发一个场景(如「帮我做一次 session handoff」)。

## Skill 一览

### 求职流水线

| Skill | 一句话 | 依赖 |
|---|---|---|
| [star-bank](star-bank/) | STAR 经历故事库——简历 / 面试 / 复盘的统一数据源 | 无 |
| [resume-tailor](resume-tailor/) | 粘贴 JD → 定制简历 bullets + 匹配度报告 + 诚实补足建议 | star-bank(必需) |
| [interview-coach](interview-coach/) | 终端模拟面试官:预测题、追问、点评,错题自动归档 | star-bank(必需) |

### Agent 工程与开源

| Skill | 一句话 |
|---|---|
| [agent-session-handoff](agent-session-handoff/) | 把 Coding Agent 会话压缩成可迁移的 handoff 文档 |
| [repo-onboarding](repo-onboarding/) | 仓库 → 带来源标注的结构化 onboarding 报告 |
| [issue-pr-triage](issue-pr-triage/) | Issue → 难度定位 + 查重 + 实现计划(需 gh CLI) |
| [companion-memory-eval](companion-memory-eval/) | 陪伴型 AI 记忆质量评测:五维度 + 错因分类 |
| [xhs-digest](xhs-digest/) | 收藏夹消化器:提炼去重,收敛成 checklist(utility) |
| content-pipeline | 带人工门禁的内容生产 SOP(7 个子技能 + 可插拔领域包)· 移植中 |

## 依赖与降级

- **star-bank 是求职三件套的数据底座**:resume-tailor / interview-coach 没有它会被 skill 拒绝执行(防止编造经历)。首次使用先按 `star-bank/templates/` 建库,条目存在你自己的本地目录(默认 `~/notes/star-bank/`,可在会话中声明其他路径),**本仓库不存任何个人数据**;
- 其余 skill 相互独立,单独拷贝即可使用;
- agent-session-handoff 的产物可直接作为下游 Agent 的首条输入,亦兼容会话检索类工具作迁移源。

## 可移植性约定

- skill 正文不含任何作者个人信息;示例一律使用虚构数据;
- 所有用户数据路径由使用者声明,仓库与云端默认无耦合;
- 平台相关能力(如 issue-pr-triage 的 GitHub 访问)统一走标准 CLI(`gh`)并注明,可替换;
- 每个 skill 的 `Boundaries` 一节声明安全红线:不编造、不越权、遇验证/异常即停。

## 编写约定

想新增或修改 skill,遵循同一骨架:frontmatter(`name` + `description`,description 必须包含触发时机与不应使用的场景)→ Start Gate → Workflow → Output Contract → Stop Gate / Boundaries。

## Origin

这套 skill 从作者的真实项目与工作流中沉淀:开源 Coding Agent CLI、AI 陪伴应用、内容运营工作台、开源社区贡献与日常开源阅读。详见[作者主页](https://github.com/zengxinyueooo)。

## License

[MIT](LICENSE)
