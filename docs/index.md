<section class="hero-section" style="text-align: center; padding: 4rem 1rem; background: linear-gradient(135deg, #e8eaf6 0%, #c5cae9 100%); border-radius: 12px; margin-bottom: 2rem; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);">
  <h1 style="font-size: 2.8rem; margin-bottom: 1rem; color: #1a237e; font-weight: 700; line-height: 1.2;">
    大模型评测方法论
  </h1>
  <p style="font-size: 1.25rem; color: #455a64; margin-bottom: 2.5rem; max-width: 600px; margin-left: auto; margin-right: auto;">
    从通用框架到 RAG 与 Agent 的工程化实践指南
  </p>
  <div>
    <a href="chapter-1/" class="md-button md-button--primary" style="margin-right: 1rem; font-size: 1rem; padding: 0.6rem 2rem; border-radius: 8px; box-shadow: 0 2px 4px rgba(0,0,0,0.2);">
      🚀 开始阅读
    </a>
    <a href="https://github.com/kaling666666/LLM-Evaluation-Methodology" class="md-button" target="_blank" style="font-size: 1rem; padding: 0.6rem 2rem; border-radius: 8px; background-color: white; color: #1a237e;">
      💻 GitHub 仓库
    </a>
  </div>
</section>

## 📖 项目简介

本白皮书旨在为“大模型评测工程师”提供一套完整的全景作战图。我们摒弃单纯的工具堆砌，从底层的**失效模式**出发，构建覆盖**安全合规、RAG质量闭环、Agent轨迹归因**的工程化评测体系。

在 AI 时代，Demo 能跑不等于系统能用。评测的意义在于将“感觉”替换为“证据”，通过分层归因、指标量化与门禁拦截，保障大模型应用在生产环境中的质量与安全。

## 🗂️ 目录导航

- **[第一章 评测总论](chapter-1.md)** 🎯
  - 明确评测的起点与决策需求，构建五层评测维度框架。
- **[第二章 RAG评测](chapter-2.md)** 🔍
  - 拆解检索与生成层，探讨基于开发周期的评测闭环与工具选型。
- **[第三章 Agent评测](chapter-3.md)** 🤖
  - 从“答案质量”走向“行为质量”，深入轨迹评测与因果归因。
- **[第四章 评测补充](chapter-4.md)** 🛠️
  - 探讨评测模型的选取、预算控制与工程化技术保障。
- **[第五章 常见问题](chapter-5.md)** 💡
  - 总结 LLM-as-Judge、RAG 与 Agent 评测中的典型坑点与解决方案。

!!! tip "阅读建议"
    建议按照章节目录顺序阅读。在实际项目中，请遵循：**先定失效模式 → 再选维度 → 再定阈值和工具 → 最后设计流水线** 的原则。
    每章末尾都附有本章小结，时间紧迫的读者可直接阅读小结部分。

---

<div style="text-align: center; color: #9e9e9e; font-size: 0.9rem; margin-top: 3rem;">
  <p>Made with ❤️ for LLM Evaluation Engineers</p>
</div>