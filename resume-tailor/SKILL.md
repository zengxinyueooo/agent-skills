---
name: resume-tailor
description: 粘贴 JD,读取 star-bank 经历库,产出定制版简历 bullets、覆盖/缺失匹配度报告和诚实的补足建议。Use when the user provides a job description (pasted text or URL) and wants a targeted resume version. Do not use for writing resumes from scratch without star-bank, fabricating experiences, or filling in application web forms.
---

# Resume Tailor · 简历定制器

一句话红线:**只重排和改写 star-bank 里的真实素材,绝不编造**。AI 改简历最大的雷是把"经历"也改了——这个 skill 的全部价值在于不踩雷。

## Start Gate

同时满足才开工:

1. 用户提供 JD 全文(或链接 + 允许抓取确认);
2. star-bank 路径已声明,且 `index.md` 可读;
3. 用户说明了目标岗位的正式名称(JD 标题 + 公司,用于输出归档)。

只有 JD 没有 star-bank → 停,先引导补库;硬要继续则只做 JD 解析,不出简历。

## Workflow

1. **解析 JD**:提取硬性要求、技术关键词、业务关键词、"潜台词"(如"快速迭代" → 讲清你的迭代节奏故事),列成关键词清单 K;
2. **匹配**:对 K 中每一项,检索 star-bank 索引,标注 `覆盖(哪条经历)` / `部分覆盖` / `缺失`;
3. **改写**:按目标岗位的权重重排模块顺序;bullets 全部从对应经历文件的「简历 bullet 版」出发做裁剪与关键词对齐,数字原样搬运;
4. **匹配度报告**:K 的覆盖率统计 + 缺失项分析;
5. **补足建议**:对缺失项,只给三类合法建议——star-bank 里可换角度挖掘的既有经历 / 一周内可做出的最小可验证补充(如给开源项目提一个相关 PR) / 明确标注"该要求当前无法满足"。

## Output Contract

固定三块输出:

```
## 定制简历(<公司>·<岗位>)
<模块化 bullets,每条行尾注 [来源: 经历id]>

## 匹配度报告
- 覆盖:x/y(列表)
- 部分覆盖:...(怎么补齐表达)
- 缺失:...(对应第 5 步建议)

## 变更记录
相对上一版(或通用版)改了什么、为什么
```

## Boundaries

- bullets 里的每个数字必须能在 star-bank 对应文件中找到,找不到的当场标红并要求用户确认,确认前不输出;
- 缺失就是缺失,报告里不美化;
- 不代填网申表格、不生成假项目描述、不输出"优化经历"类建议;
- 输出按 `<公司>-<岗位>/` 归档到用户的求职工作区,方便海投时版本管理。
