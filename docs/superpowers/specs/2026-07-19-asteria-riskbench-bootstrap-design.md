# Asteria RiskBench 启动设计：治理基线与最小证明纵切

- 日期：2026-07-19
- 状态：设计第 1–6 部分均已获得用户批准
- 系统：Asteria RiskBench / Asteria 个人风险工作台
- 仓库：`J:\asteria-riskbench`
- 远端：`https://github.com/everything-is-simple/asteria-riskbench.git`

> **正式分册入口**：`docs/design/01-系统治理与文档权威链.md` 至 `docs/design/06-TDD-验证证据与首个纵切验收.md`。
>
> 本文件保留为启动设计聚合总览和六部分审批形成证据。日常引用应进入对应正式分册；如本文件摘要与正式分册或批准记录冲突，以正式分册和批准记录为准。

## 1. 设计目标

建立一个个人可维护、离线优先、本地数据优先、默认只读、风险优先的 ETF 风险观察工作台。首个纵切优先证明数据合同、MALF 结构语义、降级门禁、审计谱系和 Viewer 只读边界，而不是追求大而全功能。

系统只发布结构事实和证据，不输出选股、排名、买卖建议、仓位、订单或 PnL。

## 2. 治理与文档权威链

### 2.1 仓库内入口

每次开工按以下顺序：

`AGENTS.md → docs/00-索引.md → docs/01-当前任务.md → 当前正式设计/合同 → 外部 Definitive 原文`

仓库文档是长期上下文。聊天、临时浏览器草图和 AI 记忆不得替代它。

### 2.2 外部权威

MALF 语义权威：

`J:\asteria-trading-labs-Definitive-validated\MALF_Definitive_v2_0-claude-20260616`

Data 合同权威：

`J:\asteria-trading-labs-Definitive-validated\Data_Definitive_v2_0-claude-20260616`

精确文件路径和 SHA-256 见 `docs/02-权威来源登记.md`。

### 2.3 参考目录治理

七个参考目录全部只读、禁止复制迁移、禁止写缓存、禁止运行时耦合。首个纵切原始数据权威 `J:\new_tdx64\vipdoc` 单独登记，同样只读。

## 3. 领域架构和边界

```text
外部只读数据
    ↓
Data Adapter / Validation
    ↓
MALF Core → Range → Lifespan → Probability
    ↓
WaveProbabilitySnapshot（月 / 周 / 日）
    ↓
MultiTimeframeRiskCoordinateSnapshot
    ↓
G0–G3 + Audit + Atomic Publication
    ↓
Local Read-only Viewer
```

### 3.1 MALF

职责：市场结构、Wave/Range 生命周期和概率位姿。

正式对外契约：`WaveProbabilitySnapshot`。

不得输出：交易信号、仓位、订单、PnL、bucket 或综合概率分。

### 3.2 立花

职责：未来的库存和标的暴露管理。

v0.1：`design-only`，不进入首个纵切。

### 3.3 个人风险

职责：未来的账户级集中度、暂停和恢复边界。

v0.1：`design-only`。

### 3.4 AI Observer

职责：未来的只读风险观察与人工复核提示。

v0.1：`not-started`。不得写回领域状态，不得覆盖硬门禁，不得生成订单。

### 3.5 UI

只读取已经发布的不可变快照。不得读取外部数据源，不得执行 MALF，不得推断 None 字段。

## 4. 首个证明纵切

首批标的：

- `510300` 沪深300ETF；
- `510500` 中证500ETF；
- `159915` 创业板ETF；
- `512880` 证券ETF；
- `513100` 纳指ETF。

数据流：

```text
J:\new_tdx64\vipdoc 原始 .day
→ TDX 只读解析
→ ETF PriceBar（raw_none / fixed-point）
→ 周/月聚合
→ MALF Core + Range
→ 多周期透明坐标
→ G0–G3 审计快照
→ 原子发布
→ 本地 Viewer
```

### 4.1 两段证明

纵切 A：Data、聚合、Core、Range、门禁、快照和 Viewer。

纵切 B：只有在 `qfq_back`、peer sample、防前视、精度合同和 Range/Lifespan/Probability TDD 全部满足后，才允许完整 Probability 发布。

当前不把纵切 B 伪装为已完成。

## 5. MALF v2.0 透明多周期坐标

月、周、日各自独立运行同一批准版本的 MALF，并分别生成不可变 `WaveProbabilitySnapshot`。

应用层组合：

```text
MultiTimeframeRiskCoordinateSnapshot
- symbol
- as_of_date
- data_freshness
- usage
- price_line
- month_snapshot
- week_snapshot
- day_snapshot
- rule_versions
- source_hashes
- reason_codes
- coordinate_completeness
```

应用层不重新分类、不输出 `bucket_id`、不合成总分、不生成交易结论。

## 6. 27 桶撤回决定

以下方案永久撤回，不进入正式模型：

```text
月线：牛 / 熊 / 盘
周线：顺 / 逆 / 盘
日线：新高 / 新低 / 盘
3 × 3 × 3 = 27 桶
```

原因：

- 没有 Definitive v2.0 依据；
- 绕过 pivot/wave/guard/break/range 状态机；
- 与 v2.0 禁止 bucket 和综合强弱分冲突。

## 7. ETF fixed-point adapter

当前 `raw_none` TDX ETF 使用来源原始整数价格比较：

- `price_scale=1000`；
- `/1000` 只用于展示；
- Pivot、Guard、Break 只使用严格 `<` / `>`；
- 相等不触发；
- 禁止 binary float；
- 禁止临时 `round(2)`；
- 周/月聚合保持相同整数价格域。

规则版本：`malf-v2.0-etf-tick-v0.1`。

该规则是 RiskBench adapter，不修改 Definitive。未来 `qfq_back` 不自动继承此规则，必须建立独立固定精度合同和审批记录。

## 8. Data 合同

### 8.1 TDX

- `.day` 记录长度：32 字节；
- 解析布局：`<5If2I`；
- 最后两个字段 unsigned；
- 文件长度必须可被 32 整除；
- 日期必须严格递增且不晚于 evaluation_date；
- OHLC 必须合法；
- 结构性坏记录整只标的拒绝；
- 禁止跳过坏 bar。

### 8.2 周/月聚合

- open：区间第一根 open；
- high：区间 max(high)；
- low：区间 min(low)；
- close：区间最后一根 close；
- volume/amount：sum；
- `bar_dt`：区间第一个交易日；
- Data 层不计算 MALF 语义。

### 8.3 当前数据资格

五只样本最后数据日均为 `2026-06-29`。评估日期为 `2026-07-19`。当前没有满足目标合同的五只 ETF `qfq_back` 数据。

因此真实快照只能：

- `price_line=raw_none`；
- `usage=research_only`；
- `freshness=stale_research_only`。

## 9. 四级门禁

### G0 输入完整性

失败即 `rejected`：截断、格式错误、重复/乱序日期、未来日期、非法 OHLC、非法价格、读取期间来源变化。

### G1 数据合同与新鲜度

新鲜度以明确交易日历和 evaluation_date 为准，不使用任意自然日阈值。stale、freshness_unknown 或价格线不满足目标合同，只能降级 `research_only`。

### G2 模型完整性

Pivot、Wave、Range、Lifespan、Probability 或 peer sample 不足时，对应字段为 `None + reason_codes`。禁止 neutral/default/fallback。

### G3 谱系与发布

缺少来源哈希、数据摘要、解析器版本、四层 MALF 版本、adapter 版本、门禁结果或内容哈希时禁止发布。

v0.1 只允许：`verification_only`、`research_only`、`rejected`。`operational` 不启用。

## 10. 审计快照

最小审计字段：

- `run_id`、`snapshot_id`、`produced_at`、`evaluation_date`；
- source path、size、mtime、SHA-256、latest_bar_date；
- parser、aggregation、core、range、lifespan、probability、adapter 版本；
- 记录数、首末日期、price_line、price_scale；
- G0–G3 结果、usage、reason_codes、warnings；
- 父快照哈希和当前内容哈希。

规范化序列化：UTF-8、键排序、ISO-8601、整数价格保持整数、Decimal 使用字符串、禁止 NaN/Infinity。

## 11. Viewer 与运行边界

采用快照优先：

```text
显式构建 → G0–G3 → 原子发布 → 本地 Viewer
```

Viewer：

- 仅绑定 `127.0.0.1`；
- GET/HEAD-only；
- 不读取 TDX；
- 不执行聚合或 MALF；
- 不写 snapshot、指针、日志或外部目录；
- 不依赖公网、遥测、云账号或远程数据库；
- 不启动 scheduler、watcher 或自动重算。

三个视图：

1. 风险概览；
2. 标的详情；
3. 审计详情。

首屏必须显示绝对日期、stale、research_only、raw_none 和 None 原因。禁止排名、推荐、买卖文案、仓位、订单和 PnL。

v0.1 不建设可写设置中心。只读显示生效 profile、逻辑数据源、标的、price_line、price_scale 和规则版本。首个 profile：`etf_raw_research_v0.1`。

## 12. 原子发布和回退

```text
var/staging/<run_id>
→ 完整校验与哈希
→ var/published/<snapshot_id>
→ 原子更新 current.json
```

- snapshot 不可覆盖；
- 新构建失败不改变 `current.json`；
- 当前快照损坏时 Viewer 停止展示，不静默扫描旧版本；
- 回退必须显式选择已验证 snapshot_id；
- 回退动作生成独立审计事件；
- `var/` 不进 Git，可删除并由相同输入和规则重建。

## 13. TDD 与验证证据

证据权威顺序：

`Definitive → approved adapter → reviewed golden fixtures → historical differential → real-data integration`

六层验证：

1. V1 Data 合同；
2. V2 周/月聚合；
3. V3 MALF 状态机；
4. V4 G0–G3 与审计；
5. V5 Viewer 与运行边界；
6. V6 确定性重放、发布和回退。

关键边界必须覆盖：

- fractal k=2 和相等不触发；
- 初始化不足保持 uninitialized；
- Guard 相等不 break；
- terminated wave 不复活；
- latest candidate replacement；
- New Wave 双条件；
- Range 边界演化与 resolution；
- Wave/Range 样本隔离；
- sample_cutoff 防前视；
- peer sample < 30 为 None；
- P1–P4 None 条件；
- reason_codes 和完整 rule_versions；
- 禁止 bucket、综合分和 fallback。

TDX 固定回归：`512880 volume_raw=2793949440` 不得解析为负数。

每条关键规则必须建立：

`权威条款 → 测试 ID → fixture → 预期事件序列 → 实际结果 → 审计证据`

## 14. 首个纵切验收结论边界

只有在 Data/Core/Range、ETF adapter、G0–G3、确定性发布、Viewer 只读性和显式回退通过完整追踪测试后，合成链路才能标记 `verified`。

五只真实过期 `raw_none` ETF 即使链路成功，也只能标记 `research_only`。

在 `qfq_back`、peer sample、Range/Lifespan/Probability TDD 和防前视资格满足前：

- Probability 保持 `None`；
- 状态为 `not_verified`；
- 不得声称 MALF v2.0 全链路已验证。

## 15. 建议的未来仓库边界

以下只是后续实施计划的目标结构，本治理提交不创建业务骨架：

```text
config/
src/asteria_riskbench/
  core/
  data/
  malf/
  application/
  audit/
  ui/
tests/
scripts/
docs/
var/                 # ignored, rebuildable
```

立花、个人风险、AI Observer 在获得各自实施门禁前，不创建空壳来制造“已经开始”的假象。

## 16. 回滚与停止条件

任何以下情况立即停止业务施工：

- 权威文件哈希变化且未重新审批；
- 参考目录发生意外写入；
- raw_none 被当作 qfq_back；
- stale 被称为当前状态；
- 27 桶、综合分或交易建议重新出现；
- UI 开始重算领域逻辑；
- 测试预期由待测实现自我生成；
- G3 谱系不完整仍发布；
- 实施超出 `docs/01-当前任务.md`。

停止后保留证据、恢复到最近已验证提交或 snapshot，并请求用户裁决。
