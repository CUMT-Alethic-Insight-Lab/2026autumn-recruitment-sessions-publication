> 脱敏展示候选；身份与私人信息已替换，保留原始表达；不代表内容经事实核验。展示编号：C2R6。以下为原会话记录，保留完整题面、角色顺序、技术推理、原有错误与自评；有关第三方产品的主张为原会话表述，不代表发布方背书。仅移除身份与精确时间。

# 会话清洗报告

- 展示编号：C2R6
- 轨迹：selected
- 开考时间：[精确时间已移除]
- 交卷时间：[精确时间已移除]
- 题号：第 1 题
- 轮次统计：user 7 · assistant 7
- 时间范围：[精确时间已移除]

## 👤 用户

你是资深 AI 系统架构师，专攻长程 agent 的上下文与状态管理。

全程遵守：
1. 只给可执行设计，拒绝形容词。每个机制必须能回答三问：
   触发条件是什么？阈值是多少？失败时怎么降级？
2. 每个结论必须标注：它解决题干里的哪个信号，或哪个失效模式。
3. 不确定就写「未决」，不许用猜测填空。
4. 涉及"总结/智能/自适应"时，必须给出具体算法或判定式。
5. 输出中文，术语保留英文原词。不写寒暄，不写"总而言之"。

## 🤖 助手

题干缺失。你要求每个结论对应「题干里的信号」和「失效模式」，但本条消息只有约束、没有问题描述；此刻输出任何设计都属于猜测填空，违反第 3 条。

需要你补齐的最小输入：

1. **信号**：题干里被点名要解决的现象（原文引用即可），及其可观测指标。
2. **失效模式**：已知的失败案例或必须防住的 failure mode 清单。
3. **预算约束**：context window 上限、单轮 token 预算、可接受的 summarization 延迟。
4. **状态载体**：可用存储（文件/DB/vector store）、是否允许跨 session 持久化、是否需要可回滚。
5. **现有 harness**：框架、模型、已有工具链（决定机制是替换还是叠加）。

拿到后我按 `机制 → 触发条件 → 阈值 → 降级路径 → 命中信号/失效模式` 五元组逐条输出，缺数据处标「未决」。

## 👤 用户

任务：拆解下面的考题。此步严禁提出任何方案。

题目：Q1 [agent] 上下文压缩的下一步——为长程任务设计一套会"结构化遗忘"的机制
上下文压缩技术早已不新鲜，本质上是趁着上下文窗口还没用完，把总结的内容作为下一个上下文窗口的起始；Kimi Code CLI 早期提出过智能回退机制，当尝试出错的时候会总结原因、退回上一个上下文节点、继续尝试；Kimi K3 官方建议让 agent 框架返回完整历史，并指出半路接替其他模型的上下文会导致质量崩塌；to do list 现在已经是家家 agent 框架中的常客，却时常出现所有任务都做完了才对 to do list 一次性全部打勾。请设计一套能够更适合长程任务的上下文压缩机制，并指出框架结构、system prompt 和模型选型须知。 


只输出以下五节：


## 1 信号表
表格：| 题干信号 | 它暴露的失效模式 | 严格表述 |
逐条覆盖题干里的每个事实性描述，一条不漏。


## 2 真问题
≤3 条，互不重叠，每条一句话。要求：换成任何具体技术栈都成立。


## 3 评分点猜测
5–7 条，按权重降序。每条写"考官在看什么"+"答成什么样算达标"。


## 4 陷阱
3 条。每条写"多数人会写成什么样"+"为什么这样会低分"。


## 5 一句话主张
≤30 字。后文所有设计必须为它服务。

## 🤖 助手

## 1 信号表

| 题干信号 | 它暴露的失效模式 | 严格表述 |
|---|---|---|
| 上下文压缩技术早已不新鲜 | 把“压缩”默认等同于摘要，创新点被误置于压缩率而非选择策略 | 若机制只改变摘要文本质量、不改变“哪些内容被保留”的判定函数，则不构成新机制 |
| 本质是趁窗口未满，把总结作为下一个窗口的起始 | 单一有损通道：summary 成为唯一记忆载体，原始证据不可寻回，误差跨窗口累积 | 压缩后系统的可回答集合等于 summary 所支撑的集合，并随窗口数增加而单调收缩 |
| Kimi Code CLI 智能回退：出错时总结原因、退回上一节点、继续尝试 | 回退粒度与回退点判据未定义；“错误原因”作为普通文本写入上下文 | 回退目标必须是未被压缩破坏的节点；失败结论须以可撤销标记进入上下文，而非作为事实沉淀 |
| Kimi K3 建议 agent 框架返回完整历史 | 上下文被当作可移植资产；完整历史被误读为“不要压缩” | 完整历史是重建的原料，不是传输格式；是否压缩与是否返回完整历史是两个独立决策 |
| 半路接替其他模型的上下文导致质量崩塌 | 隐式状态（格式假设、tokenizer、推理惯性）与模型绑定，handoff 时丢失 | 跨模型接替必须走显式重建协议并带校验，否则该 handoff 不可用，与上下文长度无关 |
| to do list 已是各家框架常客 | 计划表示的普及被当作状态跟踪已生效；todo 可能只是装饰性产物 | todo 的存在不证明状态被跟踪，需证明其写入与动作同源 |
| 所有任务做完才一次性全部打勾 | 状态批量补写，更新时刻滞后于动作时刻 | 若状态更新时刻不等于动作发生时刻，该状态对中途决策的互信息为零，无法用于纠偏或压缩判据 |
| 要求“更适合长程任务” | 单次压缩质量被当作评估目标，跨窗口稳定性无指标 | 评估对象是跨 N 个窗口的任务完成率与信息可寻回率，不是单次摘要相似度 |
| 要求指出框架结构、system prompt、模型选型须知 | 只交算法不交落地，三者互相脱节 | 三个交付物各自需有可检查项，且同一条机制在三处表述一致 |

## 2 真问题

1. 带宽受限时，“丢弃什么”必须由可解释的选择函数判定并保留寻回路径，否则任何压缩都是不可逆的信息损失。
2. 进度、计划、失败原因等状态必须与产生它的动作同源写入，事后补写会使其对中途决策失去价值。
3. 上下文是特定模型与实现的产物，跨窗口或跨模型只能按协议重建，不能按字节搬运。

## 3 评分点猜测

| 权重 | 考官在看什么 | 达标长什么样 |
|---|---|---|
| 25% | 是否存在“遗忘的选择函数”，而非压缩率调参 | 答案给出输入特征到保留、降级、丢弃的分类规则与优先级，并能对一条具体 span 判出归属 |
| 20% | 状态与动作的事务一致性 | 答案说明状态何时写、写什么、如何对账；状态变更与 tool call 同一事务，禁止批量补写，含中断后的修复路径 |
| 15% | 可逆性与降级 | 答案中压缩产物带指针或 tombstone，取回失败时有保守降级路径（重读原文或降低任务粒度） |
| 15% | 分层结构与迁移阈值 | 答案给出层数、每层准入与驱逐规则、容量阈值与迁移目标，且迁移可逆 |
| 10% | 失败归因的隔离 | 错误结论被标记为假设而非事实，撤销时可级联清理依赖它的下游状态 |
| 10% | 跨模型与跨 session 的重建协议 | 答案说明 handoff 传什么（事件日志而非完整对话）、如何重建、如何做冒烟校验 |
| 5% | system prompt 条款与模型选型判据 | prompt 条款可被违反检测；选型给出可测指标（长上下文有效利用率、tool-call 稳定性），而非“窗口大” |

## 4 陷阱

1. **把结构化遗忘做成多级摘要，或反向照抄“返回完整历史”主张不压缩。** 前者没有丢弃判据与寻回路径，后者把迁移失败误读为长度问题；两者都绕开了“结构化”三个字。
2. **堆架构：hot/warm/cold 加 vector store 加 graph 加 reflection，却无阈值、无降级。** 机制答不出触发条件、阈值、失败路径三问，第 3 节各评分点全部落空，且与题干点名的回退、崩塌无对应。
3. **把 todo 一次性打勾当作提示词问题，写“要求模型及时更新”。** 本质是写入时机与事务边界问题，提示词无法约束更新时刻，等于未触及信号。

## 5 一句话主张

遗忘是带索引的显式决策；状态写入与动作同事务。

## 👤 用户

基于：{{上一步}}


给出三个方案，在"记忆表示"这一层必须互斥：
A 线性摘要式  B 全量回放+检索式  C 分层状态机式


每个方案严格按此格式，字数均等（每节 ≤80 字）：
- 核心机制（一句话）
- 数据结构（字段级，给出字段名与类型）
- 压缩触发（具体阈值）与降级路径
- 回退策略（退到哪个节点，如何归因）
- 对"todo 最后一次性打勾"的解法
- 崩溃条件（什么输入会让它失效）
- token 成本量级（相对全量历史的比例）


约束：不许评价优劣，不许互相借鉴，不许出现"结合两者优点"。

## 🤖 助手

## A 线性摘要式

- 核心机制：单一滚动摘要，旧片段折叠进摘要正文，原始轮次只保留指针。
- 数据结构：`SummaryDoc{segments:Segment[],cursor:int}`；`Segment{id:str,text:str,covers:[int,int],sig:str,ptr:str}`。
- 压缩触发：占用 >70% window 折叠最旧 3 段，目标比 0.2；仍 >85% 再压一档并置 `lossy=true`。
- 回退策略：退回最早未折叠段的 `ptr` 重放原文；归因写入 `Segment.why` 并置 `suspect`，禁并入 `sig`。
- todo 解法：勾选须作为 `Segment` 提交并带 `last_action_id`；与上条动作 id 不连续即拒写。
- 崩溃条件：需逐字原文、或大量互不相关短事实、或摘要二选一产生事实冲突。
- token 成本：约 20%，单窗口稳定 15–25%，随窗口数缓慢上升。

## B 全量回放 + 检索式

- 核心机制：只追加的事件日志，每步按检索得分加时间窗临时拼上下文。
- 数据结构：`Event{id:str,ts:int,kind:enum,payload:json,refs:str[]}`；`Index{postings:Map,vec:f32[n][d]}`；`Cursor{last_id:str}`。
- 压缩触发：拼装 >60% budget 把 top-k 由 40 降至 12；仍超则仅留 `goal/decision/error` 加最近 5 条。
- 回退策略：重放到 `event.id`，状态由 fold 确定性重建；归因写成新 `error` 事件，不删旧事件。
- todo 解法：todo 由 `kind=todo_*` 事件 fold 得出；两次勾选间无 `tool_result` 即判异常并回滚该批。
- 崩溃条件：日志无限增长致检索退化；因果跨度超 top-k；payload 含不可序列化大对象。
- token 成本：单步 25–35%，只随 k 与窗口变、不随历史增长；存储 100%。

## C 分层状态机式

- 核心机制：任务建模为有限状态机，每状态只留 schema 槽位，离态丢弃自由文本。
- 数据结构：`FSM{state:enum,slots:Map<str,Slot>}`；`Slot{val:any,src:str,conf:f32,ttl:int}`；`Transition{from,to,guard,why}`。
- 压缩触发：进入新状态把旧态文本压成 slots，schema 外字段丢弃；单态 >50% window 按 `ttl` 淘汰。
- 回退策略：退到上一状态快照，guard 失败写 `Transition.why`；只重放本态事件，不跨态归因。
- todo 解法：todo 即当前态 `open_subgoals` 槽；迁移 guard 要求该集合为空，批量勾完仍卡在 guard。
- 崩溃条件：任务无法预分解为状态；出现 schema 外新目标；guard 谓词不可判定。
- token 成本：约 10–15%，随状态数线性，每状态近似常数。

## 👤 用户

你现在是两个人：① 一个看过 500 份答卷的挑剔考官；② 一个正在 on-call 的生产环境负责人。不再是设计者。


对象：{{上一步}}


第一部分 判决表
| 方案 | 致命伤 | 可修复缺陷 | 不可修复缺陷 | 判决(淘汰/保留/需杂交) |


第二部分 反例推演
对每个方案，逐条推演下列四个场景会发生什么（各 2 句，要具体到字段/函数）：
1. 任务跑到第 200 轮，第 3 轮的一条约束才被用到
2. 刚过压缩边界，立刻需要 40 轮前的某段工具原始输出
3. 中途从模型 X 切到模型 Y
4. 第 80 轮被注入一个错误前提，直到第 120 轮才暴露


第三部分 一句话判决
每个方案一句，指出它最该被保留的那一个机制。

## 🤖 助手

## 第一部分 判决表

| 方案 | 致命伤 | 可修复缺陷 | 不可修复缺陷 | 判决 |
|---|---|---|---|---|
| A 线性摘要式 | 摘要改写后无法判定“第 3 轮约束是否仍在”，`sig` 只存哈希，`query()` 只扫 `segments.text` | `lossy` 标记；todo 提交校验；折叠顺序可换 | 无按约束建索引的能力，长程只能顺序扫描且扫的是被改写文本 | 淘汰 |
| B 全量回放+检索式 | `assemble()` 按 top-k 取，不沿 `refs` 做因果闭包，关键事件在日志里但不在上下文里 | 换 `Index`；k 与 recency 权重可调；embedding 可重算 | 日志与 `fold()` 的全量重放成本随轮数线性，无上界；on-call 无法给出延迟 SLO | 保留（换检索器） |
| C 分层状态机式 | schema 封闭：第 3 轮约束若不在 `Slot` 定义内，离态即丢且 `Transition.why` 不记录 | `ttl`、状态粒度、故障态可加 | 状态集合是设计期固定的，长程任务的相变不可预先枚举 | 需杂交 |

## 第二部分 反例推演

**A 线性摘要式**

1. 第 3 轮落在 `segments[0].covers=[1,50]`，约束若未被 `sig` 收录，`query()` 扫不到、不会触发 `resolve(ptr)`。第 200 轮模型从未见过该约束，且没有任何信号提示它存在。
2. 刚折叠完，目标轮次在 `covers` 内，`resolve(ptr)` 依赖外部日志仍在；日志 GC 后返回空。降级只能重跑工具，有副作用时即为数据事故。
3. `Segment.text` 是 X 的措辞，`sig` 哈希对 Y 无意义，Y 只能靠 `ptr` 重放原文。等于绕过摘要回到全量历史，压缩收益归零。
4. 错误前提被写进 `Segment.text` 并在后续折叠中继承，暴露后只能对单段置 `suspect`。引用它的 `sig` 与下游段没有反向索引，无法级联清除。

**B 全量回放+检索式**

1. `e3{kind=constraint}` 与第 200 轮 query 词面无交集，`Index.postings` 排不进 top-12。`assemble()` 不做 `refs` 闭包，e3 缺席且无告警。
2. `Event.payload` 原样保留，按 `ts` 或 `refs` 可精确取回；但 `assemble()` 已用满 k，取回需一次预算外 fetch。当前协议没有这个函数入口，属于设计缺口而非参数问题。
3. 日志与模型无关，Y 用同一 `fold()` 重建状态；但 `Index.vec` 是 X 的 embedding，不重算则 top-k 系统性失真。
4. 暴露时写 `e120{kind=error,refs=[e80]}`，`fold()` 按 `refs` 计算有效集，把 e80 标 `invalidated` 并重算派生状态。这是三个方案里唯一能级联否证的结构。

**C 分层状态机式**

1. 约束若已定义为 `slots["constraints"].val`，则随迁移复制、第 200 轮可读；若 schema 无此字段，离态时按规则丢弃，`Transition.why` 不记录丢弃项。
2. 工具原始输出不在任何 `Slot.val` 内，离态即丢；`Slot.src` 只存事件 id，而事件不在 C 的存储里，`src` 是悬空指针。
3. `slots` 的 `conf` 是 X 自评，Y 无校准基线；`guard` 谓词若依赖 X 的判定习惯，迁移直接判 false 并卡死。
4. 错误前提落在某 `Slot.val`，暴露后需手改 slot 并重跑该状态；下游 slot 无 `src` 链回溯到该事件，级联不完整。

## 第三部分 一句话判决

- A：保留 `covers+ptr` 的区间指针回退，它让“退到第 N 轮”成为可定位操作。
- B：保留只追加 `Event` 加确定性 `fold()`，它是唯一能对错误前提做级联否证的结构。
- C：保留 `Transition.guard` 对 `open_subgoals` 的准入检查，它把“批量勾完再离开”变成不可表达的操作。

## 👤 用户

基于判决，产出最终设计。

规则：
- 选一个方案做骨架，明确写出为什么它是骨架。
- 允许杂交，但每处杂交必须写明：引入哪个字段/函数、替代掉原骨架的哪个部件、
  被替代的机制为什么不够用。
- 禁止"综合各方优点"这类空话。指不到具体字段的杂交视为无效。


输出：
## 1 组件清单
表格：| 组件 | 职责（一句话）| 输入 | 输出 | 失败降级 |
## 2 数据流
文字版，从"一轮对话结束"到"下一轮窗口渲染完成"的完整路径，标出每步的判定条件。
## 3 遗忘策略表
| 记忆类型 | 保留形式 | 半衰期 | 召回方式 | 丢弃后果 |
## 4 触发器阈值表
| 触发器 | 条件 | 动作 | 降级 |
至少含：软压缩、硬压缩、阶段提交、错误事件、定期审计。
## 5 状态卡 schema
给出完整字段与必填约束，标注哪些字段必须逐字保留。

## 🤖 助手

## 1 组件清单

骨架选 B（全量回放+检索式）。理由：只有 append-only `Event` 提供无损事实源，`fold()` 是唯一能在不改写历史的前提下表达“e80 已失效”的位置；A 的摘要与 C 的 slot 都是派生视图，错了无法回溯。骨架被替换的三个部件见 §1 后的注。

| 组件 | 职责（一句话） | 输入 | 输出 | 失败降级 |
|---|---|---|---|---|
| `EventLog` | 只追加持久化全部事件，分配 `id` 与 byte offset | `Event` | `(id, offset)` | 写失败进 WAL，重试 3 次后置 `read_only=true` 并告警，agent 继续但标 `degraded` |
| `Validator` | 校验必填字段与 payload 可序列化 | raw event | `accept` / `reject(reason)` | 校验器异常时按 `kind=unknown` 落盘，不阻塞主流程 |
| `Guard` | 写前准入检查，拒绝非法状态转移 | candidate `Event` + `DerivedState` | `allow` / `reject` | 谓词抛异常时拒绝写入并升级人工，不默认放行 |
| `Indexer` | 增量维护 `postings` 与 `vec` | `Event` | `Index` delta | lag > 200 条置 `index_stale=true`，`Assembler` 退回 ts 窗口 |
| `Folder` | 确定性重放，计算 todo、invalidated 集 | `Event[]` | `DerivedState` | `state_hash` 不一致时回退最近 `Checkpoint` 重放 |
| `ClosureExpander` | 从 seed 沿 `refs` 与 constraint 扩到不动点 | `seed_ids, budget` | 有序 `event_ids` | 触预算上限按 `priority` 截断并置 `closure_truncated=true` |
| `Assembler` | 渲染上下文窗口 | `event_ids + StateCard` | prompt blocks | 超预算时降为最近 N 条加 `goal/decision/error` |
| `CheckpointStore` | 存 `StateSnapshot` 与事件 offset | `DerivedState` | `Checkpoint` | 恢复失败从 `e0` 全量重放并计时告警 |
| `Invalidator` | 错误事件级联否证 | `error Event` | `invalidated` 集增量 | `refs` 成环或缺失时只标直接引用，写 `unresolved_refs` |
| `Auditor` | 定期查悬空 `src`、未闭合 todo、manifest 偏差 | `EventLog + DerivedState` | `AuditReport` | 超时改抽样；连续 3 次失败冻结写入 |

**三处替换**

1. `Assembler` 的纯 top-k 选择器被 `ClosureExpander` 替代。不够用的原因：`e3{kind=constraint}` 与第 200 轮 query 词面无交集时排不进 top-12，上下文里没有但它存在，等价于丢失，且无告警。
2. `todo` 的事后异常检测被 `Guard` 替代。不够用的原因：fold 检测发生在写入之后，只能追加补偿事件，批量勾选已经发生；`Guard` 在 `append` 之前拒绝，使该状态不可表达。
3. `fold()` 从 `e0` 全量重放被 `CheckpointStore` 的 `Checkpoint{at_event, state_hash}` 加区间重放替代。不够用的原因：每次回退 O(n)，200 轮后无延迟上界，on-call 给不出 SLO。

## 2 数据流

1. 回合结束，产出 `Event[]`，每条 `kind ∈ {assistant_msg, tool_call, tool_result, todo_*, decision, error, phase_advance}`。判定：数组非空才进入下一步。
2. `Validator` 检查必填与体积。判定：payload > 64KB 且不可内联时外置为 blob，事件只留 `payload_ref`。
3. `Guard` 仅对 `todo_close` 与 `phase_advance` 生效。判定：`check()` 返回 false 时写 `guard_reject` 并把理由回灌模型，本回合不推进。
4. `EventLog.append()` 返回 `id` 与 offset，WAL fsync 后视为提交。
5. `Indexer` 异步增量更新。判定：lag > 200 置 `index_stale`。
6. `Folder` 增量 fold。判定：`state_hash` 不匹配则从最近 `Checkpoint` 重放，重放超 5s 则告警。
7. `Invalidator`。判定：本批含 `kind=error` 时对 `refs` 求闭包，标 `invalidated` 并重算依赖的 `DerivedState`。
8. 触发器按序判定：有 error 走错误事件；`event_count % 200 == 0` 走审计；预估占用 ≥ 88% 走硬压缩，≥ 70% 走软压缩；`phase_advance` 通过时走阶段提交。
9. `Assembler` 组 seed：当前 query 的 top-k、未闭合 `open_subgoals` 的 `src`、当前 phase 的全部 `constraints`。判定：seed 为空则退化为最近 5 条。
10. `ClosureExpander` 扩至不动点，预算 ≤ 45% window。判定：超预算按 `priority` 升序截断并置 `closure_truncated`。
11. 排序键 `(priority, ts)` 后渲染，`StateCard` 逐字字段先注入并断言哈希一致。判定：哈希不符则拒绝渲染，降级为 checklist 模式。
12. 写 `render_manifest{ids, tokens, truncated}`，供下一轮审计比对实际注入与预期。

## 3 遗忘策略表

| 记忆类型 | 保留形式 | 半衰期 | 召回方式 | 丢弃后果 |
|---|---|---|---|---|
| 用户约束、目标 | 全文 `Event`，`priority=P0` | ∞ | seed 强制注入，不经检索 | 直接违约且事后不可检测 |
| 决策与理由 | 全文 `Event`，`P0` | ∞ | priority 注入加 `refs` 闭包 | 重复决策、前后自相矛盾 |
| 错误与失效归因 | 全文加 `invalidated` 标记 | ∞ | 暴露时强制注入，屏蔽失效前提 | 错误前提复活，污染下游 |
| 工具原始输出 | 外置 blob 加 `payload_ref` | 3 天或 20 个 phase | 按 `refs`/`ts` 精确取，不走 top-k | 需重跑工具；有副作用即为事故 |
| 中间推理冗文 | 摘要句加 `ptr` | 10 轮 | 仅 top-k 召回 | 局部重推理，可接受 |
| 失败尝试轨迹 | `attempt{goal, why_failed, ptr}` | 50 轮 | 同 `goal` 再次出现时注入 | 同一坑重复踩 |
| todo、进度 | 不单独存，由 `fold()` 生成 | 0（每轮重算） | `StateCard` 注入 | 日志损坏则无法重算 |

## 4 触发器阈值表

| 触发器 | 条件 | 动作 | 降级 |
|---|---|---|---|
| 软压缩 | 预估占用 ≥ 70% 且 < 88% window | `tool_result` 正文换 `payload_ref` 加首 200 token；`assistant_msg` 折一句加 `ptr` | 处理完仍 ≥ 80% 则升级硬压缩 |
| 硬压缩 | 预估占用 ≥ 88% | 未被执行引用的 `P2` 以下事件移出窗口但不删日志；`P0` 全集强留 | `P0` 超 50% window 时按 phase 过滤，未到 phase 的约束只留 id 加一句 |
| 阶段提交 | `Guard.check(phase_advance) == true` | 写 `Checkpoint` 加新 `StateCard`，旧 phase 的 `tool_result` 半衰期置 0 | 谓词不可判定则不提交，写 `phase_blocked` 并注入未闭合项 |
| 错误事件 | 出现 `kind=error` 或 `Invalidator` 有传播 | 重算 `DerivedState`，受害事件 id 强制进下一轮 seed | `refs` 闭包超 500 条只注入直接引用加摘要，标 `closure_truncated` |
| 定期审计 | 每 200 事件或每 30 分钟 | 查悬空 `src`、未闭合 todo、`render_manifest` 偏差、index lag | 超时改抽样；连续 3 次失败冻结写入转人工 |
| 回退触发 | 同一 `goal` 连续失败 2 次 | 退到该 `goal` 起始前的 `Checkpoint` 加 delta 重放 | checkpoint 损坏则从 `e0` 重放并计时 |

## 5 状态卡 schema

```
StateCard {
  card_version:      string                    // required, 定值 "1.0"
  task_id:           string                    // required
  phase:             enum{plan,exec,verify,blocked}  // required
  goal_verbatim:     string                    // required, 逐字保留，禁止改写与截断
  constraints: [                               // required, 至少 1 条
    { id: string,
      text_verbatim: string,                   // 逐字保留
      src_event: string,                       // required, 外键
      added_at: int }
  ]
  decisions: [                                 // required, 可为空数组
    { id: string,
      choice_verbatim: string,                 // 逐字保留
      rationale: string,                       // 可摘要
      src_event: string,
      invalidated: bool }
  ]
  open_subgoals: [                             // required, fold() 生成，禁手写
    { id: string,
      desc: string,
      status: enum{open,done,blocked},
      src_event: string,
      closed_by_event: string|null }           // 与 status=done 双向约束
  ]
  known_errors: [                              // required
    { id: string,
      premise_verbatim: string,                // 逐字保留
      refs: string[],
      invalidated_by: string|null }
  ]
  artifacts: [                                 // optional
    { name: string, ref: string, sha256: string }
  ]
  budget: { window_tokens: int, used_tokens: int, soft_pct: int, hard_pct: int }  // required
  degraded:          bool                      // required
  unresolved_refs:   string[]                  // required, 空则 []
  card_hash:         string                    // required, 逐字保留，渲染前后各校验一次
}
```

必填约束：`goal_verbatim`、`constraints[].text_verbatim`、`decisions[].choice_verbatim`、`known_errors[].premise_verbatim`、`card_hash` 五处禁止摘要、截断、改写；渲染器在注入前后各算一次哈希，不一致即拒绝渲染。`open_subgoals` 只接受 `fold()` 输出，`Guard` 在 `phase_advance` 时断言其 `status=done` 全集闭合，否则 `reject`。

## 👤 用户

把最终设计写成答卷。固定章节，不得增删：

1. 一句话主张（放最前）
2. 架构图（ASCII，≤20 行，标注数据流向）
3. 分层与结构化遗忘（用 P4 的表）
4. 压缩算法（伪代码 ≤25 行，含对账与机械渲染降级）
5. 回退算法（伪代码 ≤20 行，含归因与重试预算）
6. Todo Ledger（JSON schema + 三条强制规则）
7. System prompt（可直接粘贴，≤30 行）
8. 模型选型表（4 行：主循环/压缩器/子agent/切换）
9. 评测指标 + 四个必过反例
10. 三个已知局限


格式约束：不写背景铺垫；不用"我们认为"；每个阈值必须是数字；
每段伪代码配一句"这段防的是什么事故"。

## 🤖 助手

## 1 一句话主张

遗忘是带索引的显式决策；状态写入与动作同事务。

## 2 架构图

```
[回合结束]
   │ Event[] (Validator: 必填 + payload≤64KB, 超出转 payload_ref)
   ▼
[Guard]──reject──>[guard_reject]──>回灌模型, 本回合不推进
   │ allow
   ▼
[EventLog.append]──WAL fsync──>[已提交, 返回 id/offset]
   │
   ├──>[Indexer]     增量 postings/vec      (lag>200 ⇒ index_stale)
   ├──>[Folder]      增量 fold ⇒ DerivedState (hash 不符 ⇒ 重放 checkpoint)
   └──>[Invalidator] error.refs 闭包 ⇒ invalidated 集
   │
   ▼
[触发判定] 占用≥70% 软压缩 | ≥88% 硬压缩 | 每200事件审计 | phase_advance 提交
   │
   ▼
[Assembler] seed = topk(query) ∪ open_subgoals.src ∪ phase.constraints
   ▼
[ClosureExpander] refs 不动点, 预算≤45% window, 超限置 closure_truncated
   ▼
[Renderer] StateCard 逐字字段 + 事件块 + render_manifest
   ▼
[下一轮窗口]
```

## 3 分层与结构化遗忘

| 记忆类型 | 保留形式 | 半衰期 | 召回方式 | 丢弃后果 |
|---|---|---|---|---|
| 用户约束、目标 | 全文 `Event`，`priority=P0` | ∞ | seed 强制注入，不经检索 | 违约且事后不可检测 |
| 决策与理由 | 全文 `Event`，`P0` | ∞ | priority 注入 + `refs` 闭包 | 重复决策、前后矛盾 |
| 错误与失效归因 | 全文 + `invalidated` 标记 | ∞ | 暴露时强制注入，屏蔽失效前提 | 错误前提复活并污染下游 |
| 工具原始输出 | 外置 blob + `payload_ref` | 3 天或 20 个 phase | 按 `refs`/`ts` 精确取，不走 top-k | 需重跑工具；有副作用即事故 |
| 中间推理冗文 | 摘要句 + `ptr` | 10 轮 | 仅 top-k 召回 | 局部重推理，可接受 |
| 失败尝试轨迹 | `attempt{goal, why_failed, ptr}` | 50 轮 | 同 `goal` 再现时注入 | 同坑重复踩 |
| todo、进度 | 不单独存，`fold()` 生成 | 0（每轮重算） | `StateCard` 注入 | 日志损坏则无法重算 |

## 4 压缩算法

```
def compress(window, W, log):
  u = estimate_tokens(window)
  if u < 0.70 * W: return window, "none"
  if u >= 0.88 * W: return hard(window, W)
  for e in window:
    if e.kind == "tool_result" and e.refs_hit == 0:
      e.view = e.payload_ref + head(e.payload, 200)
    elif e.kind == "assistant_msg" and e.priority <= P2:
      e.view = one_line(e) + e.ptr
  if estimate_tokens(window) >= 0.80 * W: return hard(window, W)
  return window, "soft"

def hard(window, W):
  keep = [e for e in window if e.priority == P0]
  if tokens(keep) > 0.50 * W: keep = phase_filter(keep)
  tail = [e for e in window if e.priority > P0 and e.refs_hit > 0]
  return sort(keep + tail, key=(priority, ts))[:budget(W)], "hard"

def render(window, card, seed):
  h0 = hash(card.verbatim_fields)
  out = card.verbatim_fields + [e.view for e in window]
  manifest = {ids: [e.id for e in window], tokens: estimate_tokens(out)}
  if hash(out.card_part) != h0 or not seed <= set(manifest.ids):
    card.unresolved_refs += list(seed - set(manifest.ids))
    return mechanical_render(card), "mech_fallback"
  return out, manifest
```
这段防的是：摘要或预算截断静默丢掉 P0 约束、且无人核对 —— 对账失败即强制机械渲染，只输出 schema 字段，不再生成自由文本。

## 5 回退算法

```
def rollback(target_id, log, budget=2):
  cp = nearest_checkpoint(log, target_id)      # at_event <= target
  st = cp.state if cp else replay(log, "e0")
  st = replay(log, cp.at_event if cp else "e0", target_id)
  if state_hash(st) == st.expected_hash:
    return st, "ok"
  cause = classify(log, cp.at_event if cp else "e0", target_id)
  if cause.kind == "premise_error":
    append(Event(kind="error", refs=cause.refs, premise=cause.text_verbatim))
    return fold(log), "invalidated"            # 归因落为事件, 不改历史
  if cause.kind == "tool_fault" and budget > 0:
    return rollback(target_id, log, budget - 1)
  append(Event(kind="rollback_blocked", refs=[target_id], why=cause.kind))
  return st, "blocked"
```
这段防的是：把失败原因写进摘要当事实继续推理，以及无界重试 —— 重试预算 2 次，超限落 `rollback_blocked` 停机而不是循环。

## 6 Todo Ledger

```json
{
  "ledger_version": "1.0",
  "task_id": "string",
  "items": [
    {
      "id": "string",
      "desc": "string",
      "status": "open|done|blocked",
      "src_event": "string",
      "closed_by_event": "string|null",
      "guard_ok": true
    }
  ],
  "fold_hash": "string"
}
```

1. `status` 只能由 `fold()` 写；`todo_close` 必须携带 `closed_by_event`，且该事件与最近一条 `tool_result` 的间隔 ≤ 1 条，否则 `Guard` 拒绝写入。
2. `phase_advance` 前 `Guard` 断言全部 `status == "done"` 且 `closed_by_event` 非空；任一 `open` 即 reject，批量勾完仍卡在该断言上。
3. 每 200 事件审计一次：`closed_by_event` 时间戳须单调、`src_event` 须存在；违例则整份 ledger 回滚到上一 `fold_hash` 并写 `audit_violation`。

## 7 System prompt

```
你是长程任务执行器。状态以外置 ledger 与 StateCard 为准，不以对话记忆为准。

硬约束
1. 收到 StateCard 后先读 goal_verbatim 与 constraints[].text_verbatim，逐字遵守，不得改写、概括或推断其含义。
2. 约束、决策、错误前提只能通过事件写入，禁止在正文里"记住"。
3. 每次 tool_call 后立即提交一条事件，不得合并多步后再补写。
4. todo 只能逐条关闭：一条 todo_close 必须紧跟对应 tool_result，间隔不超过 1 条事件，禁止批量关闭。
5. open_subgoals 非空时不得声明阶段完成；被 Guard 拒绝时按拒绝理由修正，不得绕过。
6. 遇到错误：写 kind=error，premise 原文照录，refs 指向被否证的事件，禁止把失败结论当作事实继续推理。
7. 只能引用上下文里出现过的 event id，不得虚构 id、文件内容、命令输出。
8. 工具输出超过 64KB 时只保留 payload_ref，需要细节时显式 fetch，不得臆测内容。
9. 摘要与原始事件冲突时，以原始事件为准，并在回复中标注冲突。
10. 收到 degraded=true 时停止扩大任务范围，只完成当前 open_subgoals。

输出格式
- 每轮结束前输出：已完成 subgoal id、下一动作、预期可验证结果。
- 不输出与当前 phase 无关的计划。

禁止
- 禁止一次性勾选全部 todo。
- 禁止在未执行工具时声称已执行。
- 禁止修改 constraints 文本与 goal_verbatim。
```

## 8 模型选型表

| 角色 | 选型判据（可测） | 硬性要求 | 不满足时 |
|---|---|---|---|
| 主循环 | 长上下文有效利用率 ≥ 60%（128k 上 32 位置探针）、tool-call 结构合法率 ≥ 99.5%、冲突指令下 P0 遵守率 ≥ 98% | 支持稳定 JSON tool call；同 prompt 两次输出 hash 漂移 ≤ 5% | 降级为短窗口 + 更频繁硬压缩，window 按 64k 计 |
| 压缩器 | 500 条 P0 事实抽取保留率 ≥ 99%、改写率 ≤ 5%、单次 ≤ 3s（输出 ≤ 8k） | 只做抽取与指针绑定，不做推理补全 | 改用规则抽取（正则 + 首 200 token），禁用模型压缩 |
| 子agent | 单任务闭环率 ≥ 90%、虚构 event id 率 ≤ 0.5%、单位成本 ≤ 主循环 1/5 | 只接收 seed 子集，不接收全量日志 | 收窄 seed 至 20 条，超限退回主循环 |
| 切换 | 同 `fold()` 重建后 20 条冒烟任务通过 ≥ 19 条 | 切换必须重算 `vec` 并校验 `state_hash`；冒烟含 1 条约束召回 | 冒烟 < 19 条即拒绝切换，回退旧模型 |

## 9 评测指标 + 四个必过反例

| 指标 | 定义 | 阈值 |
|---|---|---|
| 约束可寻回率 | 第 N 轮仍能取回第 3 轮 P0 约束的比例 | ≥ 99% |
| 状态滞后度 | 动作到 todo 写入的事件间隔 | 中位数 0，P99 ≤ 1 |
| 跨模型重建通过率 | 切换后冒烟任务通过比 | ≥ 95% |
| 错误前提存活轮数 | 暴露到失效的轮数 | ≤ 1 |
| 压缩损失 | P0 保留率 / P2 保留率 | ≥ 99% / ≤ 60% |
| 每轮 token 占比 | 渲染 token / 全量历史 | 20%–35% |

四个必过反例：

1. 第 200 轮用到第 3 轮约束 —— `constraints` 进 seed，不经 top-k；取不到即 fail。
2. 刚过压缩边界要 40 轮前工具原始输出 —— 经 `payload_ref` 精确 fetch，命中且不重跑工具；重跑即 fail。
3. X→Y 中途切换 —— `fold()` 重建 + 重算 `vec`，20 条冒烟 ≥ 19 通过；少一条即 fail。
4. 第 80 轮错误前提、第 120 轮暴露 —— `e80` 标 `invalidated`，第 121 轮 seed 不再含其派生结论；仍在即 fail。

## 10 三个已知局限

1. 日志线性增长，存储与 index 重建无上界；超过 10^6 事件需分片，分片后跨片 `refs` 闭包的 P99 延迟未决。
2. `phase_filter` 生效时（P0 集 > 45% window），跨 phase 的约束会被压成 id + 一句，此时丢失不可检测，只能靠审计抽样发现。
3. StateCard 无字段承载的任务（开放式探索、无明确 subgoal）会退化为 checklist 模式，todo 语义失效，无法给出完成判据；该场景的替代判据未决。

## 👤 用户

审阅 {{上一步}}，不要重写全文。

## 1 自评表
| 检查项 | 分(1-5) | 依据 |
检查项：三问是否全答 / 是否有具体阈值 / 是否有失败降级 / 是否可复现 /
是否有可召回路径 / 是否有一句话主张贯穿 / 是否回应了题干的每个信号。


## 2 最弱三处
每处写：问题 → 补丁（可直接替换原文的一段话）→ 补丁后的新风险。


## 3 极限压缩
如果只能保留 500 字交给评委，写出这 500 字。

## 🤖 助手

## 1 自评表

| 检查项 | 分(1-5) | 依据 |
|---|---|---|
| 三问是否全答 | 4 | §4/§5 有触发与阈值，§1 组件表有降级；但 `ClosureExpander` 只出现在数据流第 10 步，无伪代码，其预算耗尽行为只有一句 `closure_truncated` |
| 是否有具体阈值 | 4 | 70%/88%/45%/200 事件/64KB/重试 2 次均可执行；但 `budget(W)`、`refs_hit`、`phase_filter` 三个符号未定义，阈值挂在未定义函数上 |
| 是否有失败降级 | 4 | 10 个组件各有一行降级；`mech_fallback` 之后任务如何继续未写，等于只降不续 |
| 是否可复现 | 3 | `state_hash`、`nearest_checkpoint` 的选择规则、`vec` 维度与索引参数均未给，换人实现会得到不同行为 |
| 是否有可召回路径 | 4 | `payload_ref` 与 `ptr` 双路径清楚；但 §3 的半衰期（3 天/10 轮/50 轮）在 §4 算法里没有任何执行代码路径 |
| 是否有一句话主张贯穿 | 3 | “遗忘显式”贯穿；`EventLog`→`payload_ref`、`todo_close` 紧邻 tool_result，但 §3 半衰期无代码即“显式”打折 |
| 是否回应题干每个信号 | 4 | 回退（§5）、跨模型崩塌（§8 第 4 行）、批量打勾（§6）、压缩不可逆（§4）均对应；“返回完整历史”被隐式处理为“日志全留、窗口按需”，未正面表态 |

## 2 最弱三处

**① §3 半衰期是装饰性字段，算法里无淘汰路径**

问题：表里写“工具输出 3 天 / 20 phase、冗文 10 轮、失败轨迹 50 轮”，但 §4 `compress()` 只按优先级与 `refs_hit` 取舍，从不读 `age`；`payload_ref` 的 GC 也没有调用点。阈值不可执行，等于把一张表贴在设计上。

补丁（替换 §4 开头两行）：

```
def compress(window, W, log, now):
  expire(log, now)                      # half_life 到期: payload_ref 置 dangling, 事件保留壳
  u = estimate_tokens(window)
  if u < 0.70 * W: return window, "none"
```

并补一行 `expire()` 语义：`age > half_life` 且 `refs_hit == 0` 时释放 blob、事件置 `dangling=true`；`age > 2 * half_life` 时强制释放，无论引用数。

补丁后的新风险：`dangling=true` 的事件仍在窗口内，模型看到 `payload_ref` 却取不到内容，会开始编造输出。需要在 §7 加一条“`dangling` 事件只能作为失败记录引用，不得推断其内容”，否则召回路径从“取不到”退化为“假装取到”。

**② §6 规则 1 的“间隔 ≤ 1 条”可被构造绕过**

问题：十次 `todo_close` 若都声称紧邻同一条 `tool_result`，按“间隔 ≤ 1”计数全部合法；`Guard` 没有独占约束，批量打勾仍可表达。

补丁（替换 §6 规则 1）：

> `status` 只能由 `fold()` 写；每条 `todo_close` 的 `closed_by_event` 必须指向一条 `tool_result`，且该 `tool_result` 在 ledger 的 `claim` 表中未被其他 `todo_close` 引用（一对一独占）。`Guard` 在写入前查 `claim`，冲突即 reject。批量勾选因 `tool_result` 数量不足而必然冲突。

补丁后的新风险：一次 `tool_result` 真的完成了多个子任务时会被误拒，导致模型拆出无意义的探针工具调用以满足独占；成本上升，且探针调用本身污染日志。

**③ §5 错误前提只在下一轮 seed 屏蔽，当前窗口与在飞子 agent 未清**

问题：`Invalidator` 在 fold 层标 `invalidated`，但当前已渲染的窗口、已派发未返回的子 agent 仍持有 e80 派生结论；第 120 轮暴露到第 121 轮生效之间，输出可能已经被污染。

补丁（§5 在 `return fold(log), "invalidated"` 前插入）：

> 立即对当前 `render_manifest` 命中 e80 派生集的事件置 `invalidated` 并强制 re-render 一次；向所有在飞子 agent 广播 `invalidate(refs)`，未在 200ms 内确认的调用直接 abort 并记为 `stale_result`。

补丁后的新风险：re-render 打乱正在进行的一轮推理，模型可能在同一轮内看到自相矛盾的两份上下文；且 abort 子 agent 会丢失已完成但未提交的工作，重复执行成本上升。

## 3 极限压缩

主张：遗忘是带索引的显式决策；状态写入与动作同事务。

骨架为 append-only EventLog，todo 与 invalidated 集全部由 fold() 重算，不改写历史。

写入：每回合事件经 Validator（payload>64KB 转 payload_ref）→ Guard（todo_close 与 phase_advance 写前拒绝）→ append 加 WAL fsync。Guard 要求每条 todo_close 独占引用一条 tool_result，故批量勾选不可表达。

压缩：占用≥70% 软压缩，tool_result 换 payload_ref 加首 200 token；≥88% 硬压缩，P0 全留，P0 自身超 45% window 时按 phase 过滤。渲染前对账：seed 取 topk 与 open_subgoals.src 与 phase.constraints 之并，沿 refs 扩到不动点、预算 45% window；manifest 缺 seed 或 card_hash 不符即机械渲染，只输出 schema 字段。

回退：最近 Checkpoint 加 delta 重放；hash 不符先归因，premise_error 落 error 事件并 fold 级联失效，tool_fault 重试预算 2 次，超限写 rollback_blocked 停机。

跨模型：传 EventLog 不传对话，重算 vec，20 条冒烟 19 条通过才允许切换。

指标：约束可寻回率≥99%，状态滞后 P99≤1 事件，错误前提存活≤1 轮，每轮 token 占全量 20–35%。

必过反例：第 3 轮约束第 200 轮仍命中；40 轮前原始输出经 ref 取回不重跑；X→Y 切换通过；e80 错误前提在 e121 不再进入 seed。
