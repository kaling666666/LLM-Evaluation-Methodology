<div align="center">

# 📚 大模型评测方法论 (LLM-Evaluation-Methodology)

**从通用框架到 RAG 与 Agent 的工程化实践指南**

[![在线阅读](https://img.shields.io/badge/📖_在线阅读-GitHub_Pages-blue?style=for-the-badge)](https://kaling666666.github.io/LLM-Evaluation-Methodology/)
[![MkDocs](https://img.shields.io/badge/Powered_by-MkDocs_Material-526CFE?style=flat-square&logo=materialformkdocs)](https://squidfunk.github.io/mkdocs-material/)
[![License](https://img.shields.io/badge/License-Apache_2.0-green?style=flat-square)](./LICENSE)
[![GitHub Actions](https://img.shields.io/badge/Deploy-GitHub_Actions-2088FF?style=flat-square&logo=github-actions)](https://github.com/kaling666666/LLM-Evaluation-Methodology/actions)

</div>

---

## 🌟 项目简介

本白皮书旨在为“大模型评测工程师”提供一套完整的全景作战图。我们摒弃单纯的工具堆砌，从底层的**失效模式**出发，构建覆盖**安全合规、RAG质量闭环、Agent轨迹归因**的工程化评测体系。

在 AI 时代，Demo 能跑不等于系统能用。评测的意义在于将“感觉”替换为“证据”，通过分层归因、指标量化与门禁拦截，保障大模型应用在生产环境中的质量与安全。

> 👉 **强烈推荐阅读在线版本**：[https://kaling666666.github.io/LLM-Evaluation-Methodology/](https://kaling666666.github.io/LLM-Evaluation-Methodology/) （支持全文搜索与深色模式）

## ✨ 核心特性

- **分层归因框架**：将 RAG 与 Agent 的评测拆解为检索层、生成层、行为层与工程层。
- **开发周期适配**：针对基线、调优、门禁、线上监控四个阶段，提供对应的工具链选型与执行策略。
- **开箱即用的工具指南**：涵盖 RAGAS, DeepEval, LlamaIndex, Phoenix, LangSmith 等主流评测工具的用途与链接。
- **避坑指南**：总结 LLM-as-Judge、RAG 幻觉排查、Agent 轨迹分析中的典型失效模式与解决方案。

## 🗂️ 内容目录

- **[第一章 评测总论](https://kaling666666.github.io/LLM-Evaluation-Methodology/chapter-1/)**：明确评测的起点与决策需求，构建五层评测维度框架。
- **[第二章 RAG评测](https://kaling666666.github.io/LLM-Evaluation-Methodology/chapter-2/)**：拆解检索与生成层，探讨基于开发周期的评测闭环与工具选型。
- **[第三章 Agent评测](https://kaling666666.github.io/LLM-Evaluation-Methodology/chapter-3/)**：从“答案质量”走向“行为质量”，深入轨迹评测与因果归因。
- **[第四章 评测补充](https://kaling666666.github.io/LLM-Evaluation-Methodology/chapter-4/)**：探讨评测模型的选取、预算控制与工程化技术保障。
- **[第五章 常见问题](https://kaling666666.github.io/LLM-Evaluation-Methodology/chapter-5/)**：总结 LLM-as-Judge、RAG 与 Agent 评测中的典型坑点与解决方案。

## 💻 本地开发与预览

如果你想在本地运行或参与贡献，请按照以下步骤操作：

1. **克隆仓库**：
   ```bash
   git clone https://github.com/kaling666666/LLM-Evaluation-Methodology.git
   cd LLM-Evaluation-Methodology
   ```
2. 安装环境依赖（建议使用虚拟环境）：
   ```bash
   pip install mkdocs-material
   ```
3. 启动本地预览服务：
   ```bash
   mkdocs serve
   ```
   在浏览器中访问 http://127.0.0.1:8000/ 即可实时预览。
4. 部署更新：
代码提交并 Push 到 main 分支后，GitHub Actions 会自动构建并发布到 gh-pages 分支。

📄 开源协议
本项目采用 Apache-2.0 协议开源。
