# RiskBench v0.1 实施路线图

- 文档状态：`draft / awaiting-roadmap-approval`
- 路线图版本：`riskbench-roadmap-v0.1-draft.1`
- 编制日期：`2026-07-19`
- 当前规划任务：`RB-PLAN-003`
- 上游设计基线：`riskbench-design-v0.1`
- 上游批准：`RB-GATE-002 / approved`
- 下一门禁：`RB-GATE-003 / waiting-for-roadmap-approval`
- 实现状态：`not-started`

> 本文是 Roadmap、结构化任务清单、依赖图和验收门禁，不是业务实现计划的批准，不授权安装依赖、创建业务骨架或编写代码。

## 1. 路线图目标

把六份正式设计中的“治理骨架 + 最小 Data/Core/Range 纵切”拆成具有稳定编号、明确依赖、设计来源、交付物、TDD 验证层级、完成定义和后续门禁的可追踪任务。

最终要证明的纵切是：

```text
只读 TDX .day
→ 原始整数 ETF PriceBar
→ G0 输入完整性
→ 周线/月线整数聚合
→ G1 新鲜度与用途
→ MALF ETF fixed-point adapter
→ Core
→ Range
→ 月/周/日不可变快照
→ G2 字段完整性
→ G3 谱系、规范化序列化和内容哈希
→ staging / published / current.json 原子发布
→ 本地 GET/HEAD-only Viewer
→ 合成链路 verified
→ 真实五只 ETF research_only
```

## 2. 范围

### 2.1 本路线图覆盖

- 最小项目与运行治理骨架；
- 可追踪的 TDD、fixture 和证据框架；
- `etf_raw_research_v0.1` profile；
- TDX `.day` 只读解析和 G0；
- 日线到周线、月线的整数域聚合；
- evaluation date、交易日历、新鲜度和 G1；
- `malf-v2.0-etf-tick-v0.1` adapter；
- MALF Core 和 Range；
- 月、周、日三个透明快照；
- Lifespan/Probability 未完成字段的 `None + reason_codes`；
- G2、G3、不可变快照、原子发布和显式回退；
- 三个只读 Viewer 视图；
- V1–V6 验证、合成 verified 和真实 research_only 验收。

### 2.2 本路线图不覆盖

- MALF Probability 的正式实现或验证；
- 用 `raw_none` 冒充 `qfq_back`；
- `operational`；
- 27 桶、bucket、综合分、排名、推荐和交易建议；
- 立花库存实现；
- 个人账户风险实现；
- AI Observer；
- scheduler、watcher、自动重算和远程通知；
- 可写设置中心；
- 仓位、订单、交易和 PnL；
- 公网部署、遥测和外部目录写入。

## 3. Roadmap 与任务实现的关系

本文件只定义任务级结构。每个任务在实施前必须另建：

```text
docs/implementation/tasks/<任务ID>-<任务名称>-任务实现计划.md
```

每份任务实现计划必须列出：

- 设计和 Definitive 条款；
- 前置条件；
- 精确写入范围；
- 非目标；
- RED / GREEN / REFACTOR 步骤；
- 每一步验证命令和预期结果；
- golden fixture 与故障注入；
- 回滚方法和停止条件；
- Definition of Done；
- 逐步勾选、证据路径和提交哈希。

Roadmap 获批不代表任何任务计划获批。没有独立任务计划批准，任何任务都不得进入 `in-progress`。

## 4. 任务状态与执行纪律

统一状态：

```text
planned
plan-drafting
waiting-for-plan-approval
approved-for-implementation
in-progress
verification
completed
blocked
rejected
rolled-back
superseded
```

硬规则：

1. 所有任务初始状态均为 `planned`；
2. 默认一次只允许一个任务为 `in-progress`；
3. 前置依赖全部 `completed` 后才能编写该任务的最终施工计划；
4. 任务计划批准后，步骤正文和验收标准不得静默修改；
5. 执行期间只允许勾选步骤、填写命令结果、证据、阻塞和提交哈希；
6. 扩大范围必须建立 `CR-xxxx` 变更记录并重新批准；
7. 没有 RED 证据不得进入 GREEN；
8. 没有测试、审计证据和提交哈希不得标记 `completed`；
9. 失败步骤和回滚记录不得从 Git 历史中删除；
10. 每个里程碑通过门禁后才能进入下一里程碑。

## 5. 里程碑总览

| 里程碑 | 名称 | 目标 | 入口依赖 | 出口门禁 | 初始状态 |
|---|---|---|---|---|---|
| `RB-M01` | 治理型项目骨架 | 建立最小可测试、可追踪、边界明确的施工载体 | Roadmap 获批 | `RB-GATE-M01` | `planned` |
| `RB-M02` | Data 与数据资格 | 证明只读解析、G0、整数聚合、新鲜度和用途分级 | `RB-M01` | `RB-GATE-M02` | `planned` |
| `RB-M03` | MALF Core/Range 纵切 | 证明 ETF adapter、Core、Range 和三周期透明快照 | `RB-M02` | `RB-GATE-M03` | `planned` |
| `RB-M04` | 门禁、审计与发布 | 证明 G2/G3、确定性快照、原子发布和显式回退 | `RB-M03` | `RB-GATE-M04` | `planned` |
| `RB-M05` | 本地只读 Viewer | 证明 Viewer 只消费已发布快照并提供三个视图 | `RB-M04` | `RB-GATE-M05` | `planned` |
| `RB-M06` | 纵切验收 | 完成合成 verified、真实 research_only 和 V1–V6 证据包 | `RB-M05` | `RB-GATE-M06` | `planned` |

## 6. 里程碑依赖图

```mermaid
flowchart LR
    M01["RB-M01 治理型项目骨架"] --> M02["RB-M02 Data 与数据资格"]
    M02 --> M03["RB-M03 MALF Core/Range"]
    M03 --> M04["RB-M04 门禁、审计与发布"]
    M04 --> M05["RB-M05 本地只读 Viewer"]
    M05 --> M06["RB-M06 纵切验收"]
```

默认按关键路径串行施工。只有 Roadmap 变更获得批准、写入范围完全分离且不会改变证据顺序时，才允许并行任务。

## 7. RB-M01——治理型项目骨架

### 目标

建立最小施工载体，但不引入旧仓库复杂度，不建立未批准子系统空壳。

| 任务 ID | 任务 | depends_on | 正式设计来源 | 主要交付物 | 验证层级 | 完成定义 | 状态 |
|---|---|---|---|---|---|---|---|
| `RB-M01-T01` | 建立最小项目、命令与写入边界 | Roadmap 获批 | D01 §8–§11；D02 §2、§7；D05 §5、§11 | 最小业务目录边界、运行入口合同、只写仓库/`var/` 规则、忽略项 | 治理前置 | 目录不含立花/个人风险/AI 空壳；外部目录零写入；可被后续任务引用 | `planned` |
| `RB-M01-T02` | 建立 TDD、fixture 与证据追踪骨架 | `RB-M01-T01` | D06 §2–§5 | 测试 ID 规则、fixture 分层、证据目录合同、RED/GREEN/REFACTOR 命令合同 | V1–V6 前置 | 可表达“权威条款→测试→fixture→预期→实际→证据”；无自生成 golden | `planned` |
| `RB-M01-T03` | 建立共享合同和研究 profile | `RB-M01-T01`,`RB-M01-T02` | D02 §4；D03 §4–§7；D05 §7 | 不可变共享类型边界、`etf_raw_research_v0.1`、五只 symbol、`raw_none`、`price_scale=1000`、规则版本合同 | V1/V3/V4 前置 | 配置只读可见、机器路径不进入浏览器存储、未出现 operational | `planned` |

### `RB-GATE-M01`

必须证明：

- 任务实现严格限制在批准目录；
- 没有复制旧代码；
- 没有未批准领域空壳；
- TDD 和证据编号可执行；
- profile 与版本合同可追踪；
- 尚未读取真实 TDX 执行业务计算。

## 8. RB-M02——Data 与数据资格

### 目标

证明 `.day` 来源身份、32 字节记录、unsigned 字段、整只标的拒绝、整数聚合和新鲜度用途分级。

| 任务 ID | 任务 | depends_on | 正式设计来源 | 主要交付物 | 验证层级 | 完成定义 | 状态 |
|---|---|---|---|---|---|---|---|
| `RB-M02-T01` | TDX 来源身份与只读稳定读取 | `RB-M01-T03` | D04 §2、§4；D06 §5 V1、§9 | 逻辑来源身份、读取前后文件身份核验、只读字节流、源文件变化拒绝 | V1 | 不写 vipdoc；读取期间变化整只拒绝；来源 SHA-256 可进入 lineage | `planned` |
| `RB-M02-T02` | `.day` 解析、不可变 PriceBar 与 G0 | `RB-M02-T01` | D04 §4；D06 §5 V1、§9 | `<5If2I` 解码、32 字节门禁、日期/OHLC/volume/amount 不变量、稳定 reason codes | V1/V4 | 截断、重复、乱序、未来日期、非法 OHLC 全部整只 rejected；`512880 volume_raw=2793949440` | `planned` |
| `RB-M02-T03` | 周线/月线整数域聚合 | `RB-M02-T02` | D03 §6；D04 §4；D06 §5 V2 | 日→周/月聚合器、首 open/max high/min low/末 close/sum volume amount、首交易日 bar_dt | V2 | 全程保持整数价格域；禁止 float/round(2)；边界 fixture 全通过 | `planned` |
| `RB-M02-T04` | evaluation_date、交易日历、新鲜度与 G1 | `RB-M02-T02`,`RB-M02-T03` | D04 §5、§9；D06 §5 V4、§10.2 | 明确交易日历合同、fresh/stale/unknown 判定、用途分级 | V1/V4 | 不使用自然日猜测；当前五只 ETF 为 `research_only / stale_research_only`；raw_none 不升级 operational | `planned` |

### `RB-GATE-M02`

必须证明：

- TDX 输入完全只读；
- G0 坏数据不被跳过；
- 512880 unsigned 固定回归通过；
- 周/月聚合保持原始整数域；
- 新鲜度基于批准交易日历和 evaluation_date；
- 当前真实数据只能 research_only。

## 9. RB-M03——MALF Core/Range 最小纵切

### 目标

严格依据 Definitive v2.0 和 ETF adapter 证明 Core、Range 及月/周/日透明组合，不实现 Probability，不重新引入 bucket。

| 任务 ID | 任务 | depends_on | 正式设计来源 | 主要交付物 | 验证层级 | 完成定义 | 状态 |
|---|---|---|---|---|---|---|---|
| `RB-M03-T01` | ETF fixed-point adapter 与严格比较 | `RB-M02-T03` | D03 §6–§7；D06 §8 | `malf-v2.0-etf-tick-v0.1`、整数比较、展示缩放边界 | V3 | Pivot/Guard/Break 只用 `<`/`>`；相等不触发；禁止 binary float 和临时 round | `planned` |
| `RB-M03-T02` | MALF Core 状态机 | `RB-M03-T01` | D03 §1、§8.1；D06 §5 V3、§6 | Pivot、Guard、Break、Wave 事件与状态，人工 golden fixtures | V3 | 每个边界事件可追踪到 Definitive 条款；无前视；预期非实现自生成 | `planned` |
| `RB-M03-T03` | MALF Range 状态机 | `RB-M03-T02` | D03 §8.2；D06 §5 V3、§6–§7 | Range 形成、延续、终止和未形成状态 | V3 | Range 只消费 Core 正式输出；状态转换有 golden 事件序列；未知保持 None | `planned` |
| `RB-M03-T04` | 月/周/日快照与透明组合 | `RB-M03-T03`,`RB-M02-T04` | D02 §5–§8；D03 §3–§5、§8–§10 | 三个独立不可变 `WaveProbabilitySnapshot`、组合 `MultiTimeframeRiskCoordinateSnapshot` | V3/V4 | 应用层只组合不重算；Lifespan/Probability 保持 None+reason；无 27 桶、总分或建议 | `planned` |

### `RB-GATE-M03`

必须证明：

- Core 和 Range 通过人工 golden fixtures；
- ETF adapter 未修改 Definitive；
- 月、周、日独立运行同一规则版本；
- 应用层不重新分类；
- Probability 明确为 `None / not_verified`；
- 不存在 bucket、综合分或交易结论。

## 10. RB-M04——门禁、审计与可逆发布

### 目标

让每个结果都能解释来源、规则、降级、内容哈希、发布过程和回退过程。

| 任务 ID | 任务 | depends_on | 正式设计来源 | 主要交付物 | 验证层级 | 完成定义 | 状态 |
|---|---|---|---|---|---|---|---|
| `RB-M04-T01` | G2 字段完整性与 reason code 注册表 | `RB-M03-T04` | D04 §6；D06 §5 V4 | 稳定机器原因码、字段级 None、禁止 fallback 的门禁 | V4 | 每个 None 有 reason_codes；peer sample<30 不生成 rank/Probability；UI 无补全空间 | `planned` |
| `RB-M04-T02` | G3 lineage、规范序列化和内容哈希 | `RB-M04-T01` | D04 §7–§8；D06 §5 V4/V6 | run/snapshot/source/rule/gate lineage、规范 JSON、SHA-256 | V4/V6 | 缺任一必要字段禁止发布；同输入同版本同 evaluation_date 得同哈希 | `planned` |
| `RB-M04-T03` | staging、published 与原子 current.json | `RB-M04-T02` | D05 §8–§9、§11–§12；D06 §5 V6 | staging 构建、不可变发布目录、原子 current 指针 | V6 | 失败构建不改变 current；snapshot_id 不覆盖；var 可删除重建且不入 Git | `planned` |
| `RB-M04-T04` | 显式回退、审计记录与确定性重放 | `RB-M04-T03` | D05 §10、§12；D06 §5 V6 | 指定 snapshot 回退、独立审计记录、重放验证 | V6 | 禁止静默回退；无效 snapshot 不切换；回退后内容与原发布哈希一致 | `planned` |

### `RB-GATE-M04`

必须证明：

- G2 未知不被伪装成 neutral；
- G3 字段和规则版本完整；
- 发布原子且不可覆盖；
- 失败不污染 current；
- 回退显式、有审计、可重放。

## 11. RB-M05——本地只读 Viewer

### 目标

提供只读取已发布快照的本地 Viewer，浏览器刷新不触发任何领域计算。

| 任务 ID | 任务 | depends_on | 正式设计来源 | 主要交付物 | 验证层级 | 完成定义 | 状态 |
|---|---|---|---|---|---|---|---|
| `RB-M05-T01` | 已发布快照读取器与本地 GET/HEAD 服务 | `RB-M04-T03` | D05 §2、§5；D06 §5 V5 | current 解析、只读 snapshot repository、127.0.0.1 服务 | V5 | 只允许 GET/HEAD；Viewer 无 TDX/MALF import 或调用；无外部目录写入 | `planned` |
| `RB-M05-T02` | 风险概览、标的详情、审计详情三个视图 | `RB-M05-T01`,`RB-M04-T01` | D05 §3–§4、§7 | 三个视图和首屏强制信息 | V5 | 显示 evaluation/latest/usage/freshness/profile/price_line/rule/reason/lineage；不重算 | `planned` |
| `RB-M05-T03` | Viewer 运行边界与故障行为验证 | `RB-M05-T02`,`RB-M04-T04` | D05 §5–§6、§11–§12；D06 §5 V5 | 刷新、缺失 current、坏 JSON、无效 snapshot、localStorage 和外部写入测试 | V5/V6 | 刷新不构建；无遥测；机器路径不进 localStorage；错误显式且不 fallback | `planned` |

### `RB-GATE-M05`

必须证明：

- Viewer 只读取已发布 JSON；
- 只绑定 127.0.0.1；
- 仅 GET/HEAD；
- 三个视图完整；
- 不存在设置中心、scheduler、watcher、自动重算或遥测；
- UI 不推断领域状态。

## 12. RB-M06——首个纵切验收

### 目标

以合成语义正确性和真实数据资格分离的方式完成 V1–V6 验收。

| 任务 ID | 任务 | depends_on | 正式设计来源 | 主要交付物 | 验证层级 | 完成定义 | 状态 |
|---|---|---|---|---|---|---|---|
| `RB-M06-T01` | 合成 golden 端到端 verified 链路 | `RB-M05-T03` | D06 §2–§10.1 | 人工输入到 Viewer 的全链路追踪矩阵 | V1–V6 | Data、聚合、Core、Range、adapter、G0–G3、发布、Viewer、回退全部有证据 | `planned` |
| `RB-M06-T02` | 五只真实 TDX ETF research_only 链路 | `RB-M06-T01` | D03 §9；D04 §5.3；D06 §10.2 | 五只 ETF 研究快照和审计证据 | V1–V6 | 显示 latest=2026-06-29、evaluation=2026-07-19、raw_none、stale_research_only、Probability None | `planned` |
| `RB-M06-T03` | 故障矩阵、重放和 v0.1 验收证据包 | `RB-M06-T02`,`RB-M04-T04` | D06 §9–§12 | G0–G3/Viewer/发布/回退故障注入、最终追踪矩阵、验收报告 | V1–V6 | 无拒绝验收项；所有提交/命令/结果可追踪；只声明合成 verified 与真实 research_only | `planned` |

### `RB-GATE-M06`

首个纵切只能按以下方式验收：

- 合成链路：可标记 `verified`；
- 当前真实五只 ETF：只标记 `research_only / stale_research_only`；
- Probability：`None / not_verified`；
- operational：禁止；
- Lifespan/Probability、立花、个人风险和 AI Observer：不因本纵切自动获得实现资格。

## 13. 任务依赖清单

```text
RB-M01-T01
├── RB-M01-T02
└── RB-M01-T03
    └── RB-M02-T01
        └── RB-M02-T02
            ├── RB-M02-T03
            │   └── RB-M03-T01
            │       └── RB-M03-T02
            │           └── RB-M03-T03
            └── RB-M02-T04
                    └──────────────┐
RB-M03-T03 ────────────────────────┴→ RB-M03-T04
RB-M03-T04 → RB-M04-T01 → RB-M04-T02 → RB-M04-T03 → RB-M04-T04
RB-M04-T03 → RB-M05-T01 → RB-M05-T02 → RB-M05-T03
RB-M04-T04 ──────────────────────────────────────────┘
RB-M05-T03 → RB-M06-T01 → RB-M06-T02 → RB-M06-T03
RB-M04-T04 ──────────────────────────────────────────┘
```

若详细任务计划发现依赖遗漏，必须先修订 Roadmap 或提交变更记录；不得在代码中形成未记录的隐式依赖。

## 14. 设计追踪矩阵

| 正式设计分册 | Roadmap 主要覆盖 |
|---|---|
| D01 系统治理与文档权威链 | M01；全部任务审批、状态、变更和写入边界 |
| D02 领域架构与子系统边界 | M01、M03、M05；领域隔离和应用层组合 |
| D03 MALF v2.0 透明多周期坐标 | M01、M02、M03、M06 |
| D04 数据质量、门禁、新鲜度与审计 | M02、M04、M06 |
| D05 最小只读工作台、运行边界与可逆发布 | M04、M05、M06 |
| D06 TDD、验证证据与纵切验收 | 全部里程碑，重点 M01、M06 |

任务实现计划必须把上述分册简称展开为实际 Markdown 链接和精确条款，不得只写“参考设计”。

## 15. 每个任务的 Definition of Ready

任务进入 `waiting-for-plan-approval` 前必须满足：

- Roadmap 中存在稳定任务 ID；
- 所有前置依赖已经 completed；
- 权威条款和设计条款明确；
- 写入文件范围明确；
- 非目标和停止条件明确；
- RED fixture 可由人工权威推导；
- 验证命令和回滚方法可执行；
- 不依赖未批准的后续能力。

## 16. 每个任务的 Definition of Done

任务标记 `completed` 前必须满足：

1. 全部批准步骤已经勾选；
2. RED 失败原因符合预期并保留证据；
3. GREEN 只实现本任务最小范围；
4. REFACTOR 后所有相关测试继续通过；
5. 故障注入和边界测试通过；
6. 无外部参考目录写入；
7. 文档、测试、代码和规则版本一致；
8. 施工进度版已记录当前状态；
9. 提交哈希、测试命令和结果已登记；
10. 工作树达到任务计划规定的干净状态；
11. 没有超出任务的顺手实现；
12. 对应任务或里程碑门禁已经通过。

## 17. 变更控制

Roadmap 批准后：

- 修正文案、链接和证据路径可作为治理修订；
- 新增任务、删除任务、重排依赖、扩大范围、改变 DoD 或启用并行必须提交变更记录并重新批准；
- 已开始任务不得通过改 Roadmap 隐藏失败；
- 被替代任务保留原记录和 supersedes 关系；
- 任何变更不得把 Probability、operational、交易功能或旧系统复制偷偷带入 v0.1。

## 18. Roadmap 批准后的第一个动作

若 `RB-GATE-003` 获得批准，下一步只能：

1. 创建 `docs/implementation/02-施工计划进度版.md`；
2. 将本 Roadmap 的 6 个里程碑和 21 个任务登记为 `planned`；
3. 编写第一个任务 `RB-M01-T01` 的任务实现计划；
4. 提交该任务计划供用户审核。

这仍不等于批准 `RB-M01-T01` 实施。

## 19. 本 Roadmap 的唯一批准点

> **是否批准 `riskbench-roadmap-v0.1-draft.1`：采用 RB-M01 至 RB-M06 六个里程碑、21 个稳定任务、逐任务实现计划、逐任务审批、TDD 证据闭环和里程碑门禁；批准后只创建施工计划进度版并编写 `RB-M01-T01` 任务实现计划，不安装依赖、不创建业务骨架、不实现代码？**
