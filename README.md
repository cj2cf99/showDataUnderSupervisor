# showDataUnderSupervisor — 公开只读行情镜像

本仓库只提供市场与证券行情的公开 JSON。数据来自私有 `cj2cf99/catchStockData` 的 GitHub Actions；本仓库不直接采集行情、不管理证券主表，也不应存储个人持仓、估值审批、凭据或原始私有档案。

## 公开数据入口

| 文件 | 说明 | Schema |
| --- | --- | --- |
| [data/market/latest.json](data/market/latest.json) | 67项市场及板块数据，附行业日涨跌/权重等指标 | `public-market-latest/1` |
| [data/stocks/latest.json](data/stocks/latest.json) | 正式监控个股最新有效报价，以 `by_security_id` 为键 | `public-stock-quotes/1` |
| [data/stocks/markets/JP.json](data/stocks/markets/JP.json) | 日本证券 | `public-stock-market/1` |
| [data/stocks/markets/US.json](data/stocks/markets/US.json) | 美国证券 | `public-stock-market/1` |
| [data/stocks/markets/DE.json](data/stocks/markets/DE.json) | 德国证券 | `public-stock-market/1` |

公开读取前缀：`https://raw.githubusercontent.com/cj2cf99/showDataUnderSupervisor/main/`，追加上述相对路径即可。2026-10-31 JST 核对：67/67市场核心项目、122/122正式个股（日本67、美国54、德国1）；数量会随正式名单变化，不应硬编码。

## 数据含义及隐私界限

- `collected_at_jst`、`observed_at_utc` 等字段是采集时刻而不是交易所认证的成交时刻。`complete_parse` 仅证明解析覆盖，不等于正式收盘验证；`quote_as_of=null` 的数据不得标为实时成交价。
- 板块 `daily_return_pct` 是已提供的上游日涨跌，不能用 `(close/open-1)*100` 冒充；部分交易日期仍需另行核实。
- 此公开库暴露监控证券的代码和名称，任何人均可下载并保留历史版本。不要复制 StockCheck 私有估值、持仓、审批和原始历史，也不要加入已废止的资金流模块。
- 由上游私有 GitHub Actions 自动提交；切勿在本库手改行情、写入私人证券主数据或安装重复的采集工作流。

## 三仓库维护入口

- [统一实施状态、待办、验证证据](https://github.com/cj2cf99/catchStockData/blob/main/docs/CURRENT_STATUS_AND_NEXT_STEPS.md)（私有，访问需授权）
- [采集侧技术设计](https://github.com/cj2cf99/catchStockData/blob/main/docs/DATA_PIPELINE_DESIGN.md)（私有）
- [Cloudflare 消费端交接](https://github.com/cj2cf99/catchStockData/blob/main/docs/CLOUDFLARE_PUBLIC_MIRROR_CONSUMER_HANDOFF.md)（私有）
- [StockCheck 证券身份与研究数据源](https://github.com/cj2cf99/stockcheck)（私有）

**截至2026-10-31**：私有采集与公开 JSON 的实际增量发布已获 GitHub 提交验证；Cloudflare 个人站行情只读 API、动态页面消费及线上部署仍需独立验收。公开仓库文件存在，不代表网站已部署。
