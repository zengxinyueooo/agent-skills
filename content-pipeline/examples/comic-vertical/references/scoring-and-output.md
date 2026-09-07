# Scoring & Output · 领域候选评分(niche-discovery 第 5 步用)

## 四类信号(分开评估,不合并成一个模糊分)

| 信号 | 含义 | 证据来源 |
|---|---|---|
| Proven demand 已证明需求 | 相关内容的互动规模 | 校验过相关性的笔记互动数(中位数 + 分布,不看单一异常值) |
| Current demand 当前需求 | 近期活跃度 | 时窗内相关内容数量与互动趋势 |
| Reusable breadth 可复用广度 | 不同讨论角度数 | 检索结果中意图互不重叠的内容簇数量 |
| Account fit 账号契合 | 与账号定位的匹配 | DOMAIN.md 账号简述 × 内容调性,人工判断为主 |

规则:名称/别名都不出现的内容已在前置校验剔除,不计入任何信号;互动/日期缺失的样本标 `unknown` 并降权,不插值。

## 结论档位

- `strong`:四信号中三强一中性,证据 ≥3 簇
- `candidate`:需求证明充分但广度或时效存疑,列出疑点
- `watchlist`:有官方信源但平台需求证据不足,留观

## 输出 schema(niche-discovery 第 6 步)

```yaml
niche: <规范名>
official: {url, status, verified_at}
signals:
  proven: {median_likes: <n|unknown>, samples: [<note-id>...]}
  current: {recent_count: <n>, note: <...>}
  breadth: <簇数>
  fit: <一句人工判断>
verdict: strong | candidate | watchlist
reason: <两行内>
rejected_neighbors: [{name, reason}]
```
