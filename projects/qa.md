---
layout: post
title: 校园智能问答助手
subtitle: 基于 RAG 和大语言模型的云南大学校园场景智能问答系统
permalink: /projects/qa/
---

## 项目概述

本项目是**大三《智能信息处理》**课程项目。灵感源于校园生活中大量重复性问题——"图书馆几点关门？""怎么选课？"。我们构建了基于**检索增强生成（RAG）**技术的校园问答助手，基于云南大学校园真实信息，准确回答各类问题。

| 指标 | 数值 |
|------|------|
| 覆盖场景 | **12** 个 |
| 知识文档 | **850+** 条 |
| 答案准确率 | **91.3%** |
| 平均响应 | **< 2s** |

## 技术方案

采用 RAG 架构：文档分块 → ChromaDB 向量存储 → 检索 top-5 → 大模型生成。

```
用户提问 → FastAPI 接口
    │
    ├─ 向量检索（ChromaDB + bge-large-zh-v1.5）
    │   └─ 召回 top-5 最相关文档片段
    ├─ 提示词组装（LangChain Prompt Template）
    └─ 大模型生成（Ollama / qwen2.5:7b）
        └─ 流式返回 → 前端聊天界面
```

## 覆盖场景

| 课程咨询 | 校园导航 | 办事指南 | 生活服务 |
| 实验室信息 | 考试通知 | 宿舍相关 | 网络服务 |
| 社团活动 | 交通出行 | 联系方式 | 表格下载 |

## 关键技术

1. **文档分块**：LangChain RecursiveCharacterTextSplitter，512 tokens/块，重叠 50 tokens
2. **向量模型**：BAAI/bge-large-zh-v1.5，C-MTEB 中文基准测试表现优异
3. **本地化部署**：Ollama 本地运行 qwen2.5:7b，无需联网，数据安全
4. **流式输出**：SSE 流式响应，打字机效果

## 标签

`RAG` `LangChain` `Python` `ChromaDB` `FastAPI` `Ollama`

## 收获

深入理解 RAG 架构——"先检索、再生成"有效缓解大模型幻觉，回答更加可靠可追溯。LangChain 模块化工具链大大降低开发门槛。
