# Adapter: OpenCLI · 小红书(参考实现)

平台适配器把 content-pipeline 的三类抽象操作映射到具体工具。本文件是参考实现:基于 [OpenCLI](https://github.com/opencli) 的小红书适配器。**使用第三方工具请遵循其自身条款与平台规则。**

DOMAIN.md 中声明 `adapter: opencli-xiaohongshu` 时,管线技能按本文件的纪律执行。

## 使用纪律(必须先读)

1. **开始前自检一次**:`opencli doctor`——确认浏览器连通与登录态;失败即停,不反复重试;
2. **确认适配器能力**:`opencli list -f json` 与 `opencli xiaohongshu search --help`(适配器命令可能随版本变化,先发现再使用);
3. **确认账号**(如适配器支持):`opencli xiaohongshu whoami -f json`;
4. **全程只读**:不点赞、不收藏、不关注、不评论、不发布、不删除;
5. **遇验证页 / 过期签名 URL / 拒绝访问 / 异常提示 → 立即停止本次操作**,报告现象,等人工处理;不自动重试、不绕过;
6. 限速自然:聚焦查询、小 limit、不穷举滚动。

## 操作映射

| 管线抽象操作 | OpenCLI 命令形态(以 `--help` 实际输出为准) |
|---|---|
| 需求验证检索(niche-discovery) | `opencli xiaohongshu search "<规范名> <意图词>" --limit 20 -f json` |
| 参考候选检索(reference-discovery) | 同上,首屏即止,导出 JSON 后本地排序去重 |
| 详情采集(note-capture) | 适配器的详情命令,取一次,持久化返回的全部事实字段 |
| 媒体下载(asset-ingestion) | 适配器的 `download` 命令,用已采集媒体清单,不重新拉详情 |

## 字段纪律

- 检索导出只提供列表级字段(标题/作者/互动/日期/URL/note ID)时,记录 `detail_status: list_only`,不猜测其余字段;
- 详情返回什么就存什么,缺的字段标 `unknown`;
- 签名 URL 会过期:去重与溯源一律用稳定 note ID,URL 只作辅助;
- 互动数与日期缺失时不编造,标 `unknown` 并在证据评估中降权。
