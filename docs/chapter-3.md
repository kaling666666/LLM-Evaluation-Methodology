# 第三章 Agent评测

## 3.1 Agent认知：从“答案质量”到“行为质量”
RAG 的评测对象是“一次检索 + 一次生成”的输出；Agent 的评测对象是一段过程——思考、工具调用、观察、再思考、最终回答构成的轨迹。这是两者的根本区别。

一个 Agent 的最终答案正确，并不等于它“会做事”：它可能选错了工具但碰巧得到了可用结果，可能循环了多次才收敛，可能调用了高风险工具但未被发现。反过来，最终答案错误也不等于整个轨迹都错了：可能前 90% 的步骤合理，只在最后一步参数填错。如果评测只看最终答案，这些信息全部丢失。

Agent 评测的核心命题因此变为：行为是否合理、过程是否可控、失败是否可归因。RAG 的“grounding”对应到 Agent 中，是“每一步的决策是否被上下文和任务目标支撑”。

## 3.2 Agent分类

Agent 的评测方案需要区分类型，因为不同类型的轨迹结构和失败模式差异很大。

### 按工具使用范式。 
单轮工具调用 Agent（一次选择工具并执行）的失败集中在“选错工具/填错参数”；
多轮 ReAct 式 Agent 的失败还包括循环、过早终止、观察误读；带规划（planning）的 Agent 还需要评估计划本身的质量，而不仅仅是执行步骤。

### 按任务类型。 
信息检索型 Agent 的核心指标是答案正确性和引用可靠性；
代码 Agent 需要额外关注代码是否可执行、是否引入回归；
操作型 Agent（如浏览器、终端）需要评估动作安全性，包括是否执行了危险操作、是否遵循了权限边界。有研究显示，即使任务成功率在 39–64% 之间，Agent 的不安全动作率仍可达到 7–33%，这意味着安全指标不能被成功率替代。

### 按部署形态。 
离线 Agent 可以在完整轨迹上做深度评估；
在线 Agent 需要在低延迟约束下做抽样评估和实时监控。两者的评测深度和工具选择不同。

## 3.3 Agent的典型开发周期

Agent 的开发周期比 RAG 多出一个“行为调优”的维度，通常表现为：

阶段一：任务定义与轨迹基线。 明确 Agent 可用的工具集、任务成功标准，跑通端到端任务，记录第一组轨迹。这一阶段的关键产出不是“成功率”，而是轨迹的可读性——如果连人都无法从轨迹中判断发生了什么，评测就无从设计。

阶段二：工具与提示调优。 调整工具描述、参数 schema、系统提示、规划策略，以轨迹级指标为反馈信号。这一阶段的常见问题是“工具选对了但参数不对”或“工具选对了但调用顺序不对”。

阶段三：回归与门禁。 将轨迹评测固化为版本对比，检查新版本是否在工具准确率、完成度、效率或安全性上退化。门禁不仅要拦截“答案变差”，还要拦截“行为变差”。

阶段四：线上可观测性与归因。 接入 Trace 采集，对生产轨迹做抽样评估和根因定位，把失败模式反馈回离线评测集。

## 3.4 Agent基于开发周期的评测流程

### 阶段一：轨迹基线评测
评测活动：跑通多工具 Agent 任务；以结构化格式记录每一步（thought / tool_call / observation / final_answer）；对同一任务多次运行（如 -k 3）以暴露不确定性；人工或自动标注轨迹中的第一个决定性失败步骤。

核心工具：LangSmith / Langfuse 用于 Trace 记录与实验管理；AgentRubric 等轨迹级评测库可以在拿到轨迹后直接计算工具准确率、参数保真度、完成度等指标。

产出：轨迹数据集 + 基线轨迹指标 + 失败步骤初步标注。

### 阶段二：工具与行为调优评测
评测活动：针对失败轨迹做分类（WRONG_TOOL / MISSING_TOOL / BAD_ARGS / LOOP / NO_COMPLETION / HALLUCINATED_TOOL），衡量每类失败的比例；对比不同 prompt/工具描述版本在同一批任务上的轨迹指标变化。

核心工具：AgentRubric 的五维指标（Tool Accuracy、Argument Fidelity、Trajectory Efficiency、Completion、Failure Taxonomy）适合这一阶段的细粒度反馈；TRACE 框架提出的“轨迹效用函数”和“分层能力评估”可以作为设计自定义指标的参考，尤其是它指出的“高分幻觉”——只靠 Pass@1 会掩盖低效、脆弱的轨迹。

产出：失败类型分布 + 版本间轨迹指标对比 + 调优结论。

### 阶段三：回归门禁
评测活动：将轨迹评测集接入 CI；设置轨迹级阈值（如工具准确率不得低于基线、完成度不得下降超过 X%）；对每次变更跑回归，阻断退化。

核心工具：LangSmith evals 或 Langfuse evals 提供数据集、judge 和实验配置能力；AgentRubric 的 report.to_json() 可直接用于 CI 门禁和日志管道。

产出：门禁通过/阻断记录 + 退化趋势。

### 阶段四：线上 Trace 评估与根因定位
评测活动：从生产 Trace 中抽样，用 LLM-based evaluator 对轨迹进行评分；对失败样本做因果归因，区分“模型决策错误”和“工程断裂”（工具超时、参数序列化错误、上下文截断等）；将新发现的失败模式回流到离线评测集。

核心工具：Phoenix / Arize 提供 Trace 级评估和 span 级评分能力，可以将 LLM-based evaluator 或代码检查附着到具体步骤上；LangSmith 的 Trace 与 evals 在同一平台内衔接。

产出：线上失败根因报告 + 评测集更新记录。

## 3.5 Agent评测工具：用途与链接

| 工具 | 主要用途 | 核心能力 | 链接 |
| :--- | :--- | :--- | :--- |
| **AgentRubric** | 轨迹级五维评测 | tool_accuracy、argument_fidelity、trajectory_length、completion、failure_taxonomy；零依赖，输出 JSON 适合 CI | [pypi.org/project/agentrubric/](https://pypi.org/project/agentrubric/) |
| **LangSmith / Langfuse** | Trace 记录 + 数据集管理 + 评估实验 | Observe + Evaluate 层；支持 judge 配置与实验对比 | [smith.langchain.com](https://smith.langchain.com/) / [langfuse.com](https://langfuse.com/) |
| **Arize Phoenix** | 开源 Trace 可观测性与评估 | OpenTelemetry-based；LLM-based eval 附着到 trace/span；支持 Agent 评估指南 | [arize.com/docs/phoenix](https://arize.com/docs/phoenix) |
| **TRACE（参考框架）** | 深度研究 Agent 的轨迹感知评测 | 分层轨迹效用、Scaffolded Capability Assessment；揭示“高分幻觉” | [ACM 论文链接](https://dl.acm.org/doi/10.1145/3774904.3792738) |
| **AgentLens** | 交互式代码 Agent 的轨迹评测 | 形式化验证 + LLM 轨迹评论 + 版本回归 | [GitHub 仓库](https://github.com/agent-lens/agent-lens-bench) |

## 3.6 Agent技术补充：轨迹评测的独特性与归因层
Agent 评测与 RAG 评测最深刻的区别在于归因的维度。RAG 的失败可以归因到“检索层”或“生成层”；Agent 的失败需要归因到 “哪一步、哪一类决策、是模型问题还是工程问题” 。

这一区别带来了三个工程要求：

第一，轨迹必须是结构化的。 自由文本日志无法支撑自动化评测。轨迹需要至少区分 thought、tool_call、observation、final_answer，并携带工具名、参数、返回状态等字段。没有结构化轨迹，轨迹评测就退化为人工看日志。

第二，评测必须包含“效率”维度。 一个 Agent 可能最终完成了任务，但用了 10 倍于最优步数。TRACE 明确将过程效率和认知质量纳入轨迹效用，而不只是最终正确性。AgentRubric 的 trajectory_length_score 用 optimal_steps / actual_steps 来衡量这一点。

第三，安全性必须作为独立维度。 任务成功率和安全合规率不是一回事。操作型 Agent 需要评估“是否执行了不应执行的动作”，多 Agent 系统还需要评估级联注入等跨 Agent 风险。把安全指标塞进“端到端成功率”中，会导致高风险行为被平均分数掩盖。

## 3.7 本章小结
Agent 评测把评测对象从“答案”推进到“行为”。它的最小可行闭环是：结构化轨迹记录 → 轨迹级五维指标 → 版本回归门禁 → 线上 Trace 归因。工具解决的是“怎么度量”，方法论解决的是“度量什么、为什么度量”——后者不能交给工具自动决定。