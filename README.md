# 大模型评测方法论 (LLM-Evaluation-Methodology)

[![MkDocs](https://img.shields.io/badge/docs-MkDocs-blue)](https://www.mkdocs.org/)
[![Material](https://img.shields.io/badge/theme-Material-purple)](https://squidfunk.github.io/mkdocs-material/)
[![License](https://img.shields.io/badge/license-Apache--2.0-green)](./LICENSE)

> 🌐 **在线阅读地址**：[https://kaling666666.github.io/LLM-Evaluation-Methodology/](https://kaling666666.github.io/LLM-Evaluation-Methodology/)

## 📖 项目简介

本白皮书旨在为“大模型评测工程师”提供一套完整的全景作战图。摒弃单纯的工具堆砌，从底层失效模式出发，构建覆盖**安全合规、RAG质量闭环、Agent轨迹归因**的工程化评测体系。

在 AI 时代，Demo 能跑不等于系统能用。评测的意义在于将“感觉”替换为“证据”，通过分层归因、指标量化与门禁拦截，保障大模型应用在生产环境中的质量与安全。

## 🗂️ 内容目录

- **第一章 评测总论**：明确评测的起点与决策需求，构建五层评测维度框架。
- **第二章 RAG评测**：拆解检索与生成层，探讨基于开发周期的评测闭环与工具选型。
- **第三章 Agent评测**：从“答案质量”走向“行为质量”，深入轨迹评测与因果归因。
- **第四章 评测补充**：探讨评测模型的选取、预算控制与工程化技术保障。
- **第五章 常见问题**：总结 LLM-as-Judge、RAG 与 Agent 评测中的典型坑点与解决方案。

## 💻 本地部署与预览

如果你想在本地运行或参与贡献，请按照以下步骤操作：

1. 克隆仓库：
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

📄 开源协议
本项目采用 Apache-2.0 协议开源。
