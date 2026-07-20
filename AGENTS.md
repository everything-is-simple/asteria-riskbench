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
5. 任务涉及 MALF 或 Data 时，再读取本文件列出的 Definitive 原文；
6. 任务进入实施规划或施工时，必须读取并遵循 [`docs/implementation/WORKFLOW-任务开发流程控制.md`](docs/implementation/WORKFLOW-任务开发流程控制.md)。

若上述文件缺失、相互冲突或当前任务不明确：停止业务施工，只允许修复治理文档或请求用户裁决。

## 1. 系统身份

- 系统名：**Asteria RiskBench**。
- 中文名：**Asteria 个人风险工作台**。
- 仓库目录：`J:\asteria-riskbench`。
- 远端仓库：`https://github.com/everything-is-simple/asteria-riskbench.git`。
- 一句话定义：使用本地市场数据，为个人交易者提供可追溯市场事实、相对暴露声明、确定性风险边界、风险笔记和可选 AI 解读的本地个人风险工作台。

系统采用三层权威：

1. **市场事实**：TDX、MALF、G0–G3 与不可变发布快照；
2. **用户声明**：相对暴露、风险边界、风险状态、笔记和行动预案；
3. **AI 解读**：只解释、总结和提醒矛盾，不修改前两层。

本系统首先回答：

- 数据来自哪里、截止哪一天、是否过期；
- 月、周、日各自处于什么 MALF 结构状态；
- 用户在明确 `as_of` 下声明了什么相对暴露和风险边界；
- 确定性规则计算出的边界状态与距离是什么；
- 哪些字段尚未形成，以及原因是什么；
- 当前结果经过了哪些规则、门禁和审计步骤。

本系统不回答：

- 应该买什么、卖什么或买多少；
- 胜率、收益预测、综合交易分；
- 券商账户、订单、成交、真实仓位或 PnL；
- 自动下单或自动交易；
- 用 AI 计算、修改或控制风险数值。

## 2. 文档和语义权威链

发生冲突时，按以下顺序裁决：

1. 用户在仓库中明确批准并提交的治理决策；
2. 本 `AGENTS.md` 的施工与安全约束；
3. `docs/02-权威来源登记.md` 中登记并校验哈希的 Definitive 文档；
4. `docs/design/01-系统治理与文档权威链.md` 至 `docs/design/07-产品重定位与每日风险闭环.md` 七份正式分册；
5. `docs/superpowers/specs/2026-07-19-asteria-riskbench-bootstrap-design.md` 聚合总览与审批形成证据；
6. `docs/03-设计批准记录.md`；
7. `docs/01-当前任务.md`；
8. 已批准的实施计划、代码与测试证据；
9. 参考仓库的历史实现和测试；
10. 临时浏览器草图、聊天记录和未提交附件。

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
- 当前比较域版本为 `source_integer_fixed_point-v0.1`；`/1000` 展示转换不得进入结构计算；
- v0.1 不读取、保存或校验交易所 `tick_size`，也不进行 tick-size quantize；
- 禁止 binary float 和临时 `round(2)`；
- Pivot、Guard、Break 使用严格 `<` / `>`，相等不触发；
- adapter 版本固定为 `malf-v2.0-etf-tick-v0.1`；
- 该 adapter 是 `raw_none / research_only` 的 RiskBench variant，不得宣称为 MALF Definitive `round(2)` 精度策略的等价实现；
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

### 8.1 市场派生数据 `var/`

```text
var/
├── staging/
├── published/
└── current.json
```

- `var/` 只保存可删除、可重建的派生数据，不进入 Git；
- Viewer 只读取 `var/published/` 与原子 `current.json` 指针，不读取 TDX，不计算 MALF；
- 发布采用不可变 snapshot；禁止静默回退，回退必须显式、可审计；
- 删除或重建 `var/` 不得触碰 `state/`。

### 8.2 用户不可再生资产 `state/`

```text
state/
├── workspace data
├── backups/
└── workbench.lock
```

- `state/` 保存用户声明、边界、风险状态、笔记、AI 解读和备份，不进入 Git；
- 所有修改追加新的不可变 revision，禁止原地覆盖；
- 写入必须事务化；损坏时停止写入，禁止静默创建空库；
- 破坏性迁移前必须备份；v0.1 只做本地备份／恢复，不做通用迁移包；
- 写入进程必须对 `state/workbench.lock` 获取操作系统级独占锁；锁文件中的 PID、启动时间和实例 ID 只用于诊断，残留文件不得永久阻塞。

### 8.3 服务与网络边界

- 外部目录全部只读；
- 市场快照 Viewer 仅绑定 `127.0.0.1`，保持 GET/HEAD-only；
- 工作站中的用户工作区可以按批准合同写入 `state/`，但不得因此让 Viewer 重算市场事实；
- 手机／平板／桌面只做浏览器设备模拟、同机窗口或自动化 viewport 响应式验收；
- 真机跨设备访问、LAN 绑定、隧道、公网、小主机和云部署在 v0.1 中禁止；
- 禁止将监听地址改为 `0.0.0.0`、局域网 IP 或公网地址；
- 禁止后台 scheduler、文件 watcher、自动重算、遥测和公网依赖；
- 不得将机器本地路径或秘密写入浏览器 `localStorage`；
- 不建设可写设置中心。

## 9. 开发方法与验收纪律

所有任务必须遵循 [`docs/implementation/WORKFLOW-任务开发流程控制.md`](docs/implementation/WORKFLOW-任务开发流程控制.md)。该文件统一规定任务登记、计划批准、TDD、逐步施工记录、验证、回滚、文档收尾、Git 交付和中断恢复；它不单独授权安装依赖、建立业务骨架或实现代码。

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

## 10. 工程技术与部署治理

涉及技术选型、开源组件、依赖安装、调试、Adapter 装配或部署演进时，必须同时遵循：

- [`docs/implementation/TECH-工程技术选型、开源组件、调试与装配治理.md`](docs/implementation/TECH-工程技术选型、开源组件、调试与装配治理.md)；
- [`docs/implementation/DEPLOYMENT-前后端边界与部署演进治理.md`](docs/implementation/DEPLOYMENT-前后端边界与部署演进治理.md)。

两份文档只建立工程治理基线，不预先批准任何具体技术、依赖、前后端框架、隧道、小主机或云部署。

当前总原则：

```text
逻辑前后端边界立即建立；
v0.1 物理部署可以先保持单机；
Viewer 只读已发布快照；
构建端与 Viewer 不互相越权；
本地 → 隧道 → 小主机 → 云端必须逐阶段触发专项设计和门禁。
```

## 11. 当前系统状态

- 治理设计第 1–6 部分：用户已批准并发布；
- `RB-GATE-004`：用户已于 2026-07-20 批准产品重定位；
- 设计第 7 部分：`approved / design-only`，作为产品范围、三层权威、`var/`／`state/` 和每日风险闭环的正式依据；
- `riskbench-roadmap-v0.2`：产品重定位后的当前 Roadmap；原 `riskbench-roadmap-v0.1-draft.1` 保留历史但已被 supersede；
- `riskbench-task-workflow-v0.2`：当前任务与里程碑治理流程；
- `RB-M01-T01`：`suspended-by-RB-GATE-004`，历史计划保留但不得执行；
- 当前 Roadmap：R0 文档重定位完成，R1 等待技术选型；
- 下一门禁：`RB-TECH-002 / waiting-for-technology-selection-approval`；
- 三份技术研究草案：保持 untracked、non-authoritative、not-selected，不得作为安装或实施依据；
- 正式业务项目骨架：未开始；
- 依赖安装：未开始；
- Data/Core/Range、个人风险、笔记、AI 和 Viewer 实现：未开始；
- 真实数据用途：`research_only`；`operational` 禁用；
- 临时审阅服务 `127.0.0.1:63840` 只作为设计审阅证据，不纳入正式运行体系。

下一步必须以 `docs/01-当前任务.md` 为准。任何 AI 不得因用户说“继续”就自行扩大范围，也不得恢复执行已暂停的 `RB-M01-T01`。

## 12. 治理文件修改规则

以下文件属于治理基线，修改前必须说明原因、影响和权威依据：

- `AGENTS.md`
- `README.md`
- `docs/00-索引.md`
- `docs/01-当前任务.md`
- `docs/02-权威来源登记.md`
- `docs/03-设计批准记录.md`
- `docs/design/01-系统治理与文档权威链.md` 至 `docs/design/07-产品重定位与每日风险闭环.md`
- `docs/superpowers/specs/2026-07-19-asteria-riskbench-bootstrap-design.md`
- `docs/implementation/00-实施计划索引.md`
- `docs/implementation/01-RiskBench-v0.1-实施路线图.md`；
- `docs/implementation/02-施工计划进度版.md`；
- `docs/implementation/tasks/` 下的任务实现计划；
- `docs/implementation/WORKFLOW-任务开发流程控制.md`
- `docs/implementation/TECH-工程技术选型、开源组件、调试与装配治理.md`
- `docs/implementation/DEPLOYMENT-前后端边界与部署演进治理.md`

不得删除历史决策来“让文档看起来一致”；冲突必须通过新增决策记录和显式 supersedes 关系解决。
