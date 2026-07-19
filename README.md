# Asteria RiskBench

**Asteria 个人风险工作台**：一个使用本地市场数据，为个人交易者发布 ETF 多周期市场结构与风险观察快照的本地只读工作台。

> 当前状态：**治理与正式设计文档基线**。尚无可运行的业务应用，尚未安装项目依赖，尚未实现 MALF 计算链路。

## 系统回答什么

- 数据来自哪里、截止哪一天、是否过期；
- 月、周、日分别处于什么 MALF 结构状态；
- 哪些字段尚未形成以及对应原因；
- 当前结果经过了哪些规则、门禁和审计步骤。

## 系统明确不做什么

- 不选股、不排名、不推荐；
- 不输出胜率、综合交易分或“27 桶”；
- 不生成买卖建议、仓位、订单或 PnL；
- 不连接券商、不自动交易；
- 不让 UI 或 AI 重新计算、覆盖硬领域规则；
- 不复制旧系统代码和复杂度。

## 每次开工

必须按以下顺序阅读：

1. [`AGENTS.md`](AGENTS.md)
2. [`docs/00-索引.md`](docs/00-索引.md)
3. [`docs/01-当前任务.md`](docs/01-当前任务.md)
4. 当前任务引用的设计与权威原文

不要依赖聊天上下文恢复系统约束。

## 当前首个证明纵切

```text
J:\new_tdx64\vipdoc（只读 TDX .day）
→ 标准 ETF PriceBar（raw_none / 原始整数）
→ 周线、月线聚合
→ MALF Core + Range
→ G0–G3 门禁与审计快照
→ 本地 GET-only Viewer
```

首批标的：

- `510300` 沪深300ETF
- `510500` 中证500ETF
- `159915` 创业板ETF
- `512880` 证券ETF
- `513100` 纳指ETF

截至 **2026-07-19**，五只样本的最后数据日均为 **2026-06-29**，因此真实结果只能是 `research_only / stale_research_only`。

## 领域边界

| 子系统 | 职责 | v0.1 状态 |
|---|---|---|
| MALF | 市场结构与概率位姿 | 首个证明纵切；Probability 资格未满足 |
| 立花 | 库存与标的暴露管理 | design-only |
| 个人风险 | 账户级生存边界 | design-only |
| AI Observer | 只读风险观察与权限边界 | not-started |
| UI | 读取已发布快照 | 设计已批准，尚未实现 |

## 数据与参考资料

- 原始行情权威：`J:\new_tdx64\vipdoc`；
- `J:\malf-data` 只做交叉核验；
- 七个历史/参考目录全部只读，禁止复制迁移；
- MALF 语义以 `J:\asteria-trading-labs-Definitive-validated\MALF_Definitive_v2_0-claude-20260616` 为权威。

完整路径、用途和 SHA-256 见 [`docs/02-权威来源登记.md`](docs/02-权威来源登记.md)。

## 正式设计

以下六份分册共同构成 **RiskBench v0.1 已批准设计基线**，也是日常引用入口：

1. [系统治理与文档权威链](docs/design/01-系统治理与文档权威链.md)
2. [领域架构与子系统边界](docs/design/02-领域架构与子系统边界.md)
3. [MALF v2.0 透明多周期坐标](docs/design/03-MALF-v2.0-透明多周期坐标.md)
4. [数据质量、分级门禁、新鲜度与审计](docs/design/04-数据质量-分级门禁-新鲜度与审计.md)
5. [最小只读工作台、运行边界与可逆发布](docs/design/05-最小只读工作台-运行边界与可逆发布.md)
6. [TDD、验证证据与首个纵切验收](docs/design/06-TDD-验证证据与首个纵切验收.md)

辅助治理材料：

- [聚合启动设计：治理、领域、纵切、门禁、Viewer 与 TDD](docs/superpowers/specs/2026-07-19-asteria-riskbench-bootstrap-design.md) — 保留为总览与审批形成证据；
- [设计批准记录](docs/03-设计批准记录.md)。

## 实施规划

用户已批准进入实施计划阶段。当前仅建立：

- [实施计划索引](docs/implementation/00-实施计划索引.md)
- [RiskBench v0.1 实施路线图](docs/implementation/01-RiskBench-v0.1-实施路线图.md)

Roadmap 当前为待审规划稿。施工计划进度版、具体任务实现计划、依赖安装、项目骨架和代码仍未授权。

## 仓库状态

远端：`https://github.com/everything-is-simple/asteria-riskbench.git`

当前只提交治理、设计和实施规划文档。后续施工计划进度版、任务实现计划、项目骨架、依赖选择和业务实现必须分别经过新的用户门禁。
