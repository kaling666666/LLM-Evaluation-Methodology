# 第二章 RAG评测

2.1 RAG认知：检索不是目的，grounding才是
RAG（Retrieval-Augmented Generation，检索增强生成）的工程直觉常被简化为“先搜再答”。这个简化在工程上足够，但在评测上是有害的：它让人误以为“检索到了”就等于“答对了”。实际上，RAG 的核心质量命题是 grounding：生成的答案是否被检索到的证据所支撑，以及检索到的证据是否足以支撑一个正确的答案。

RAG 的评测因此天然是分层的。检索层的问题（“正确片段在不在 Top-K 里”）和生成层的问题（“答案有没有超出检索上下文”）必须分开度量。把两者混为一个端到端分数，会丢失归因能力，使调优变成盲目的 prompt 试错。

2.2 RAG分类：按检索范式与生成策略划分
RAG 系统的评测方案需要与其架构类型匹配。常见的分类维度包括：

按检索器类型。 稀疏检索（BM25）强在精确词匹配，弱在语义泛化；稠密检索（向量检索）强在语义相似，弱在罕见实体和精确术语；混合检索在两者之间取平衡。评测检索层时，指标的选择与检索器类型无关（Hit-Rate、MRR 通用），但基线预期不同——稀疏检索的 Hit-Rate 对措辞敏感，稠密检索的 Failure 更多来自 embedding 的领域漂移。

按生成策略。 抽取式（从上下文中抽取答案）的 Faithfulness 通常更高，但覆盖率受限；生成式（基于上下文自由生成）的 Answer Relevancy 可能更高，但幻觉风险上升。评测时应根据生成策略调整 Faithfulness 的阈值严格程度。

按知识更新模式。 静态索引的 RAG 和持续更新的 RAG 在回归评测策略上不同：前者只需在发布前跑全量评测，后者需要将索引变更纳入门禁触发条件。

2.3 RAG的典型开发周期
RAG 的开发不是线性的“建索引 → 跑问答”，而是围绕“检索质量”和“生成质量”交替迭代的循环。一个可操作的周期划分如下：

阶段一：基线与数据准备。 确定被测数据集、标注或合成评测集（问题 + ground truth 上下文/答案），跑通原始检索与生成链路，得到第一组基线指标。

阶段二：检索层调优。 调整 chunk_size、top_k、embedding 模型、reranker、检索策略（稀疏/稠密/混合），以检索层指标（Hit-Rate、MRR、Context Precision/Recall）为反馈信号。

阶段三：生成层调优。 在检索稳定的基础上，调整 prompt、生成参数、引用策略，以 Faithfulness 和 Answer Relevancy 为反馈信号。

阶段四：可观测性与回归固化。 接入 Trace 工具，将评测集和阈值固化为回归门禁，使后续任何变更（模型、prompt、索引）都经过同一套质量检查。

2.4 RAG基于开发周期的评测流程
阶段一：基线评测
评测活动：构造或标注评测集；对基线 RAG 系统跑全量指标；记录每个样本的检索结果和生成结果。

核心工具：LlamaIndex 的 RetrievalEvaluator（用于独立评估检索器，支持 Hit-Rate、MRR 等排序指标）；RAGAS 或 DeepEval 用于生成层指标。

产出：基线指标表（检索层 + 生成层），以及可复现的评测脚本。

阶段二：检索层调优评测
评测活动：对 chunk_size × top_k × embedding/reranker 组合做参数网格搜索；每一组配置只关注检索层指标，隔离生成层的变化。

核心工具：LlamaIndex RetrievalEvaluator 负责 Hit-Rate / MRR；如果使用 AutoRAG 类工具做自动化搜索，其底层评估逻辑仍然应落到同一组检索指标上。

产出：参数-指标对比表，明确“检索层的瓶颈在哪里”以及“当前配置离检索上限有多远”。

阶段三：生成层调优与端到端验证
评测活动：在选定检索配置上，评估 Faithfulness、Answer Relevancy、Answer Correctness；对失败样本做逐条归因（检索未召回 / 检索召回了但生成未使用 / 生成使用了但推理错误 / 引用标记错位）。

核心工具：RAGAS 的 faithfulness / answer_relevancy / context_precision / context_recall；DeepEval 的 ContextualRecall、CitationFaithfulness 等指标，适合需要逐条解释的工程化场景。

产出：生成层指标 + 失败归因表 + 是否达到上线阈值的结论。

阶段四：可观测性与回归门禁
评测活动：对线上或预发 Trace 进行抽样评估；将评测集接入 CI，设置指标阈值，任何低于阈值的变更加以阻断；记录 Trace 链路图作为归因证据。

核心工具：Phoenix / Arize 提供 Trace 采集、LLM-based evaluation、以及将评估结果附着到 span 的能力；DeepEval 和 RAGAS 的指标可以集成到 Phoenix 的评估流程中。

产出：Trace 证据、门禁阻断记录、回归通过率趋势。

2.5 RAG评测工具：用途与链接
工具	主要用途	核心能力	链接
RAGAS	RAG 全链路指标评测	faithfulness、answer_relevancy、context_precision、context_recall、answer_correctness；与 MLflow 集成	https://docs.ragas.io/
DeepEval	工程化 RAG 评测与 CI 集成	ContextualRecall、CitationFaithfulness 等自解释指标；阈值与 strict_mode 适合门禁	https://deepeval.com/
LlamaIndex Evaluation	检索层独立评测 + 生成层评估	RetrievalEvaluator 提供 Hit-Rate、MRR、Precision、Recall、NDCG；响应评估覆盖 Correctness、Faithfulness、Relevancy	https://developers.llamaindex.ai/
TruLens	RAG 评估 + 可观测性	RAG Triad（Context Relevance、Answer Relevance、Groundedness）；Feedback Functions	https://www.trulens.org/
Arize Phoenix	Trace 级可观测性与评估	基于 OpenTelemetry 的 Trace 采集；LLM-based eval 附着到 trace/span；支持 RAGAS、DeepEval 集成	https://arize.com/docs/phoenix
选型建议：如果目标是快速得到指标全貌，RAGAS 的指标集最完整；如果需要把评测嵌入 CI 并做门禁，DeepEval 的阈值与 strict_mode 设计更工程友好；如果检索层是主要瓶颈，先用 LlamaIndex RetrievalEvaluator 把检索问题从生成问题中拆出来；如果已经进入可观测性阶段，Phoenix 提供从 Trace 到评估的闭环。

2.6 RAG技术补充：RAG vs. 长上下文 vs. Markdown结构化供给
RAG 的评测方案设计，需要理解它在技术选型中的位置。长上下文模型和 Markdown 结构化供给正在改变 RAG 的适用边界，但并没有取消 RAG 的评测需求，而是改变了评测的侧重点。

RAG vs. 长上下文。 长上下文在“文档总量小、更新频率低、全局推理要求高”的场景中有质量优势。一项 2026 年的案例研究报告称，长上下文在制造安全培训问答上的正确率为 73.1%，而语义 RAG 为 65.4%，但长上下文的每查询 token 成本是 RAG 的 26 倍。另一项对比也指出，长上下文在资源充足时平均质量更高，而 RAG 的成本显著更低。这意味着 RAG 的评测不能只测“质量上限”，还必须同时把成本与延迟作为一等指标纳入决策。

RAG vs. Markdown 结构化供给。 Markdown 常被用作 Agent 的“技能手册”或规则文件的格式，它与 RAG 解决的是不同问题：RAG 补知识，Markdown 技能文件教行为。Markdown 的优势在于可读、可版本管理、可按需加载，模型不需要一次性读取全文。但这并不意味着 Markdown 可以替代 RAG：面对大规模、持续更新、需要精确检索和溯源的知识库，RAG 仍然是工程上更可行的选择。在评测上，如果一个系统同时使用 RAG 提供事实知识、使用 Markdown 提供操作规则，那么评测必须分别覆盖知识 grounding 和行为遵循，不能用一个“端到端正确率”掩盖两者的独立失效。

2.7 本章小结
RAG 评测的核心方法是分层归因：检索层回答“找得对不对”，生成层回答“答得有没有依据”，端到端层回答“对用户好不好”。工具的选择服务于这个分层结构，而不是反过来用工具的全量指标替代思考。长上下文和 Markdown 供给是技术选型的变量，它们改变 RAG 的边界和成本结构，但不改变“grounding 必须被度量”这一根本要求。