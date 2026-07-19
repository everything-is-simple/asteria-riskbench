# 设计第 6 部分：TDD、验证证据与首个纵切验收

- 文档状态：`approved / design-only`
- 基线版本：`riskbench-design-v0.1`
- 批准日期：`2026-07-19`
- 上游设计：分册 02–05
- 下一阶段：获得独立批准后编写实施计划；本分册不授权业务实现

## 1. 验证目标

RiskBench 不以“页面看起来合理”“真实行情走势合理”或覆盖率百分比证明正确性。

每条关键不变量必须建立：

```text
权威条款
→ 测试 ID
→ 人工审阅 fixture
→ 预期事件序列
→ 实际结果
→ 审计证据
```

只有通过完整追踪的能力才能标记 `verified`。

## 2. 证据权威顺序

```text
Definitive 原文
→ 已批准 RiskBench adapter/profile
→ 人工审阅 golden fixtures
→ 历史实现差分
→ 真实数据集成
```

约束：

- Definitive 是语义权威；
- RiskBench adapter 只能解决明确批准的本地适配；
- golden 预期不得由待测实现自动生成；
- 历史实现只能用于差分，不得覆盖权威语义；
- 真实市场数据只证明集成，不证明所有边界语义。

## 3. TDD 闭环

### RED

- 先把权威条款转换成最小失败测试；
- 明确输入 fixture、预期事件和失败原因；
- 确认测试失败是因为能力尚未实现，而不是测试环境错误。

### GREEN

- 只实现让当前测试通过的最小逻辑；
- 不顺手加入排名、缓存、后台任务、AI 或其他未批准能力；
- 不扩大当前实施计划的文件范围。

### REFACTOR

- 测试保持通过后再整理结构；
- 不改变合同、字段语义、事件顺序和错误行为；
- 补充文档和验证证据。

禁止先实现再补测试。

## 4. 测试与证据命名

实施计划应采用稳定命名空间：

- `V1-DATA-*`：TDX 和 PriceBar 合同；
- `V2-AGG-*`：日/周/月聚合；
- `V3-MALF-*`：Core/Range/Lifespan/Probability 状态机；
- `V4-GATE-*`：G0–G3 和审计；
- `V5-VIEW-*`：Viewer 和运行边界；
- `V6-PUB-*`：确定性发布、重放和回退。

每个测试证据至少记录：

- authority reference；
- fixture identity 和 hash；
- evaluation date；
- rule versions；
- expected events/result；
- actual events/result；
- pass/fail；
- evidence artifact identity。

## 5. 六层验证矩阵

### V1——Data 合同

验证：

- TDX 32 字节记录；
- `<5If2I`；
- unsigned 字段；
- 日期合法、严格递增、无未来日期；
- OHLC/volume/amount 不变量；
- 截断、重复、乱序和源变化整只拒绝；
- 不跳坏 bar；
- PriceBar 不可变；
- 原始整数和 price_scale 保持。

### V2——周/月聚合

验证：

- open 为区间第一根；
- high 为最大值；
- low 为最小值；
- close 为区间最后一根；
- volume/amount 求和；
- bar 日期采用批准的周期合同；
- 输入和输出保持相同整数价格域；
- 不经过 binary float；
- 不混入未来 bar。

### V3——MALF 状态机

验证 Core、Range，并为未来 Lifespan/Probability 建立资格门禁：

- fractal `k=2`；
- 相等不触发 pivot；
- 初始化历史不足保持 uninitialized；
- Guard 相等不 break；
- up/down wave 的 guard 与 break；
- terminated wave 不复活；
- latest candidate replacement；
- New Wave 双条件；
- Range 边界演化和 resolution；
- Wave/Range 样本隔离；
- sample_cutoff 防前视；
- peer sample `<30` 时相关 rank/Probability 为 None；
- P1–P4 不满足时为 None；
- 缺失规则版本时不能发布。

### V4——门禁与审计

验证：

- G0 失败产生 `rejected`；
- G1 根据交易日历和 evaluation date 判断；
- stale/raw_none/freshness_unknown 降为 `research_only`；
- G2 字段级 None + reason_codes；
- 禁止 fallback 和默认 neutral；
- G3 缺来源哈希、lineage 或规则版本时禁止发布；
- 规范化 JSON 和内容哈希稳定；
- v0.1 不出现 operational。

### V5——Viewer 与运行边界

验证：

- GET/HEAD-only；
- 只绑定 `127.0.0.1`；
- 页面刷新前后 snapshot 文件、hash 和 mtime 不变；
- Viewer 不读取 TDX；
- Viewer 不执行聚合或 MALF；
- stale、research_only、raw_none、绝对日期和 None 原因首屏可见；
- 无排名、推荐、仓位、订单、PnL；
- 无外部网络请求；
- 当前快照损坏时不自动查找旧快照。

### V6——确定性重放、发布与回退

验证：

- 同一 fixture、evaluation date 和规则版本连续运行两次，规范化 JSON 与 SHA-256 完全相同；
- staging 中断或哈希不符不改变 current.json；
- 同一 snapshot_id 不可覆盖；
- 原子替换失败不留下半发布状态；
- 显式回退只能指向已验证快照；
- 回退产生独立审计事件；
- 禁止静默 fallback。

## 6. Core/Range 关键不变量

实施计划必须把 Definitive 条款精确映射到测试，不得只使用本摘要：

- pivot 候选窗口和严格比较；
- 初始 wave 的形成条件；
- guard 更新；
- break 与终止；
- 候选 pivot 替换；
- 新 wave 创建条件；
- transition 到 Range 的边界；
- Range 持续、边界演化、突破与 resolution；
- 终止对象不可复活；
- 事件序列和最终快照必须同时验证。

## 7. Lifespan/Probability/Service 资格边界

- Wave 与 Range 使用独立 lifespan 样本；
- `sample_cutoff` 不得包含 evaluation point 之后的数据；
- peer sample 少于 30 时 rank 为 None；
- P1–P4 相关字段不满足输入条件时保持 None；
- 禁止用 fallback 补足 Probability；
- `WaveProbabilitySnapshot` 是唯一对外 MALF 快照；
- 快照不可变，并包含完整 reason_codes 和 rule_versions。

当前阶段不允许把 Probability 标记为 implemented 或 verified。

## 8. ETF fixed-point adapter 专项证明

必须覆盖：

- 来源整数 `1123` 展示为 `1.123`，结构比较仍使用 `1123`；
- 构造 `1.123` 与 `1.124`，证明 `round(2)` 会破坏顺序；
- 相同整数价格不触发严格突破；
- 日/周/月聚合保持同一 price_scale；
- 全链路不经过 binary float；
- `raw_none` adapter 通过不代表 `qfq_back` 精度合同已验证；
- 未来 qfq_back 使用独立 fixture、规则版本和审批记录。

## 9. TDX 回归与故障注入

固定覆盖：

- `512880 volume_raw=2793949440`，不得解析成负数；
- 31/33 字节尾部；
- 重复日期；
- 乱序日期；
- 未来日期；
- 非法 OHLC；
- 零或负价格；
- 读取期间文件变化；
- 任一坏记录触发 G0 rejected；
- 结果中不得存在删除坏记录后的成功快照。

## 10. 首个纵切验收分级

### 10.1 合成链路 `verified`

只有在以下能力全部通过追踪矩阵后，才能标记合成链路 `verified`：

- Data；
- 周/月聚合；
- Core；
- Range；
- ETF fixed-point adapter；
- G0–G3；
- 确定性发布；
- Viewer 只读性；
- 显式回退。

### 10.2 真实数据 `research_only`

五只 ETF 能完成只读解析、聚合和研究快照发布后，仍必须显示：

- latest bar date `2026-06-29`；
- evaluation date `2026-07-19`；
- `raw_none`；
- `stale_research_only`；
- Probability None 和原因。

### 10.3 Probability `not_verified`

在 qfq_back、peer sample、Range/Lifespan/Probability TDD 和防前视资格满足前：

- Probability 保持 None；
- 状态保持 `not_verified`；
- 不得声称 MALF v2.0 全链路已验证。

## 11. 拒绝验收的情况

- 只报告覆盖率，没有条款到证据的追踪矩阵；
- golden 预期由待测实现生成；
- 用真实走势“看起来合理”代替严格边界；
- 把历史实现通过写成 Definitive v2.0 已实现；
- 把 raw_none、过期数据或 Probability=None 包装成 operational；
- 重新引入 27 桶、综合分、交易建议或默认 neutral；
- UI 测试允许重算领域逻辑；
- 缺 G3 谱系仍发布。

## 12. 本分册验收条件

- 六层验证矩阵均有明确边界；
- 证据权威顺序固定；
- 每条关键不变量可追踪到测试 ID；
- Data、MALF、门禁、Viewer、发布和回退均有故障注入；
- 合成 verified 与真实 research_only 明确分离；
- Probability 资格不足时保持未验证；
- 本文不被解释为已授权编写代码。

## 13. 下一门禁

六份正式设计分册提交并由用户审核后，下一批准点才是：

> 批准进入实施计划阶段，只编写精确文件、测试、命令、提交点和回滚步骤；该批准不等于批准安装依赖或开始业务实现。

实施计划获得再次批准后，才能建立项目骨架和执行 TDD 实现。后续另建“施工计划进度版”持续记录计划项、依赖、状态、验证证据和提交哈希。
