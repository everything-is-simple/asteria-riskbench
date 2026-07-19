# AGENTS.md — Asteria RiskBench 仓库级操作宪章

> 适用范围：`J:\asteria-riskbench` 及其全部子目录。
>
> 目标：任何 AI、开发者或自动化工具在对话中断后，只读本文件与文档入口即可恢复系统身份、权威来源、当前任务和禁止事项。不得依赖聊天记忆代替仓库文档。

## 0. 每次开工的强制入口顺序

在读取或修改任何业务文件之前，必须依次阅读：

1. `AGENTS.md`；
2. `docs/00-索引.md`；
3. `docs/01-当前任务.md`；
4. 当前任务在索引中列出的权威设计、合同和决策记录；
5. 任务涉及 MALF 或 Data 时，再读取本文件列出的 Definitive 原文。

若上述文件缺失、相互冲突或当前任务不明确：停止业务施工，只允许修复治理文档或请求用户裁决。

## 1. 系统身份

- 系统名：**Asteria RiskBench**。
- 中文名：**Asteria 个人风险工作台**。
- 仓库目录：`J:\asteria-riskbench`。
- 远端仓库：`https://github.com/everything-is-simple/asteria-riskbench.git`。
- 一句话定义：使用本地市场数据，为个人交易者发布可追溯的 ETF 多周期市场结构与风险观察快照的本地只读工作台。

本系统首先回答：

- 数据来自哪里、截止哪一天、是否过期；
- 月、周、日各自处于什么 MALF 结构状态；
- 哪些字段尚未形成，以及原因是什么；
- 当前结果经过了哪些规则、门禁和审计步骤。

本系统不回答：

- 应该买什么、卖什么或买多少；
- 胜率、收益预测、综合交易分；
- 自动下单、生产仓位或 PnL；
- 用 AI 替代硬风险边界。

## 2. 文档和语义权威链

发生冲突时，按以下顺序裁决：

1. 用户在仓库中明确批准并提交的治理决策；
2. 本 `AGENTS.md` 的施工与安全约束；
3. `docs/02-权威来源登记.md` 中登记并校验哈希的 Definitive 文档；
4. `docs/superpowers/specs/2026-07-19-asteria-riskbench-bootstrap-design.md`；
5. `docs/03-设计批准记录.md`；
6. `docs/01-当前任务.md`；
7. 参考仓库的历史实现和测试；
8. 临时浏览器草图、聊天记录和未提交附件。

说明：MALF 的领域语义以 Definitive v2.0 为最高权威；`AGENTS.md` 负责规定如何安全施工，不得改写 MALF 领域含义。

## 3. MALF 权威定义：必须读取的绝对路径

MALF 正式全称：**Market Analysis Logical Framework**。

权威目录：

`J:\asteria-trading-labs-Definitive-validated\MALF_Definitive_v2_0-claude-20260616`

任务涉及 MALF 时，必须按顺序读取：

1. `J:\asteria-trading-labs-Definitive-validated\MALF_Definitive_v2_0-claude-20260616\MALF_00_Bridge_v2_0-claude-20260616.md`
2. `J:\asteria-trading-labs-Definitive-validated\MALF_Definitive_v2_0-claude-20260616\MALF_01_Core_v2_0-claude-20260616.md`
3. `J:\asteria-trading-labs-Definitive-validated\MALF_Definitive_v2_0-claude-20260616\MALF_02_Range_v2_0-claude-20260616.md`
4. `J:\asteria-trading-labs-Definitive-validated\MALF_Definitive_v2_0-claude-20260616\MALF_03_Lifespan_v2_0-claude-20260616.md`
5. `J:\asteria-trading-labs-Definitive-validated\MALF_Definitive_v2_0-claude-20260616\MALF_04_Probability_v2_0-claude-20260616.md`
6. `J:\asteria-trading-labs-Definitive-validated\MALF_Definitive_v2_0-claude-20260616\MALF_05_Service_v2_0-claude-20260616.md`
7. `J:\asteria-trading-labs-Definitive-validated\MALF_Definitive_v2_0-claude-20260616\MANIFEST-claude-20260616.json`

文件 SHA-256 固定在 `docs/02-权威来源登记.md`。目录名中的 `Definitive-validated` 表示权威文档集合，不得推断为 v2.0 Range、Lifespan、Probability 已有完整运行实现。

不得把以下旧文件提升为 MALF v2.0 权威：

`J:\asteria-trading-lab\docs\malf\Asteria-MALF-系统专属定义-v0.1.md`

它只能作为旧 Trading Lab 研究适配边界的参考。本仓库不复制该文件，避免制造第二份漂移的 MALF 定义。

## 4. 七个只读参考目录

以下七个目录只允许读取、检索和人工对照：

1. `J:\asteria-trading-lab`
2. `J:\asteria-trading-labs-data`
3. `J:\asteria-trading-labs-Definitive-validated`
4. `J:\asteria-trading-labs-reference`
5. `J:\malf-data`
6. `J:\malf-system-history`
7. `J:\tdx_offline_Data`

强制规则：

- 禁止修改、删除、移动、重命名或格式化其中任何文件；
- 禁止在其中运行会生成缓存、日志、数据库、测试产物或锁文件的命令；
- 禁止整体复制、目录迁移、代码搬运或以旧仓库为模板克隆复杂度；
- 禁止直接 import、软链接或运行时依赖旧仓库内部模块；
- 允许提取已批准的领域事实，但必须在新仓库中重新实现并记录权威来源；
- 历史代码与 Definitive 冲突时，以 Definitive 为准；
- `J:\malf-system-history` 仅作历史实现和失败经验的证据库，不是新系统代码来源。

## 5. 数据权威源

### 5.1 原始行情权威

首个纵切的原始 TDX 行情权威源是：

`J:\new_tdx64\vipdoc`

它不计入上面的七个“参考目录”，因为它是运行输入的数据权威源；但仍然必须只读。

### 5.2 交叉核验源

- `J:\malf-data`：只用于交叉核验，不得覆盖或替代 TDX `vipdoc` 原始记录；
- `J:\tdx_offline_Data`：历史/替代副本，只用于调查差异，不是首个纵切的数据裁决源；
- `J:\asteria-trading-labs-data`：历史派生数据参考，不是当前原始行情权威。

发生差异时必须：

1. 保留各来源身份、日期范围和哈希；
2. 报告差异，不静默择一；
3. 原始 `.day` 字段以 `J:\new_tdx64\vipdoc` 为准；
4. `malf-data` 只能提供交叉核验证据。

### 5.3 当前已知数据资格

首批标的：`510300`、`510500`、`159915`、`512880`、`513100`。

截至 2026-07-19 的核验结果：五只 ETF 的最后数据日均为 `2026-06-29`，且当前可用价格线为 `raw_none`。因此真实数据只能发布为：

- `usage=research_only`；
- `freshness=stale_research_only`。

不得称为“当前风险状态”，不得升级为 `operational`。

## 6. 已批准的 MALF / ETF 硬约束

- 透明坐标由月、周、日三个独立 `WaveProbabilitySnapshot` 组合；应用层只组合，不重算；
- 永久撤回“27 桶”作为 MALF 正式模型；
- 禁止 `bucket_id`、综合分、胜率、买卖建议、仓位、订单和 PnL；
- 当前 TDX `raw_none` ETF 使用来源原始整数价格严格比较；
- `price_scale=1000` 只用于展示；
- 禁止 binary float 和临时 `round(2)`；
- Pivot、Guard、Break 使用严格 `<` / `>`，相等不触发；
- adapter 版本固定为 `malf-v2.0-etf-tick-v0.1`；
- 该 adapter 不修改 Definitive 原文；未来 `qfq_back` 必须另立精度合同并重新审批；
- TDX `.day` 记录长度为 32 字节，解析布局为 `<5If2I`，最后两个字段按 unsigned 读取；
- 结构性坏记录必须整只标的拒绝，禁止跳过坏 bar。

## 7. 分级门禁与使用等级

- G0：输入完整性失败即 `rejected`；
- G1：数据合同与新鲜度决定用途降级；
- G2：模型不完整时字段为 `None + reason_codes`；
- G3：lineage、规则版本或审计不完整时禁止发布。

v0.1 只允许：

- `verification_only`；
- `research_only`；
- `rejected`。

`operational` 明确禁用，必须通过未来独立审批。

## 8. 运行和写入边界

- 外部目录全部只读；
- 新系统只允许写入仓库自身受控文件，以及运行期 `var/`；
- `var/` 可删除、可重建，不进入 Git；
- Viewer 只读取已发布快照，不读取 TDX，不计算 MALF；
- Viewer 仅绑定 `127.0.0.1`，保持 GET/HEAD-only；
- 禁止后台 scheduler、文件 watcher、自动重算、遥测和公网依赖；
- 不得将机器本地路径或秘密写入浏览器 `localStorage`；
- 不建设可写设置中心；
- 发布采用不可变 snapshot 和原子 `current.json` 指针；
- 禁止静默回退，回退必须显式、可审计。

## 9. 开发方法与验收纪律

实现必须遵循 TDD：

1. RED：先写与权威条款对应的失败测试；
2. GREEN：只写使当前测试通过的最小实现；
3. REFACTOR：测试保持通过后再整理结构。

证据顺序：Definitive → 已批准 adapter → 人工 golden fixtures → 历史差分 → 真实数据链路。

禁止：

- 先实现再补测试；
- 用待测实现自动生成自己的 golden 预期；
- 用真实行情“看起来合理”证明语义正确；
- 仅以覆盖率百分比验收；
- 把历史实现通过写成 MALF v2.0 全链路已验证。

每条关键不变量必须建立：

`权威条款 → 测试 ID → fixture → 预期事件序列 → 实际结果 → 审计证据`

## 10. 当前系统状态

- 治理设计第 1–6 部分：用户已批准；
- 仓库治理基线：已于 2026-07-19 提交并发布到 `origin/main`；基线提交为 `bbdd6d3315d23288ee4f62e138dcf044f40a49fa`；
- 当前门禁：等待用户审核正式设计并明确批准进入实施计划；
- 正式业务项目骨架：未开始；
- 依赖安装：未开始；
- Data/Core/Range 实现：未开始；
- Lifespan/Probability 实现：未开始；
- 立花、个人风险、AI Observer：`design-only` / `not-started`；
- 真实数据用途：`research_only`。

下一步必须以 `docs/01-当前任务.md` 为准。任何 AI 不得因用户说“继续”就自行扩大范围，也不得把“等待批准”解释成已经获准编写实施计划。

## 11. 治理文件修改规则

以下文件属于治理基线，修改前必须说明原因、影响和权威依据：

- `AGENTS.md`
- `README.md`
- `docs/00-索引.md`
- `docs/01-当前任务.md`
- `docs/02-权威来源登记.md`
- `docs/03-设计批准记录.md`
- `docs/superpowers/specs/2026-07-19-asteria-riskbench-bootstrap-design.md`

不得删除历史决策来“让文档看起来一致”；冲突必须通过新增决策记录和显式 supersedes 关系解决。
