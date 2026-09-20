# AI 学习项目合集

个人 AI 大模型应用开发学习仓库，包含 RAG 检索增强生成与 LangChain 核心特性两大模块。

## 仓库内容

| 目录 | 内容 |
|------|------|
| 根目录 | **RAG 检索增强生成** — 基于 LangGraph + Milvus 的混合检索 RAG 实践 |
| [`langchain_study/`](langchain_study/) | **LangChain 系统学习** — Agent、记忆系统、Guardrails、HITL、MCP 等练习代码 |

---

# RAG 检索增强生成

基于 **LangGraph + Milvus** 的 RAG（Retrieval-Augmented Generation）实践项目，从基础检索问答到自适应 RAG（Adaptive RAG），完整学习检索增强生成的核心流程与工程实现。

## 项目亮点

- **混合向量检索**：Milvus 稠密向量（HNSW）+ 稀疏向量（BM25）混合检索
- **自适应 RAG 工作流**：自动路由问题走向（向量库 / Web 搜索），文档相关性评分、幻觉检测、答案质量评估闭环
- **Markdown 文档解析**：结构化解析 Markdown 标题层级，写入 Milvus 向量库
- **对话式 Agent**：基于 LangChain Tool Calling Agent，支持多轮对话历史
- **多模型支持**：兼容 OpenAI 协议（GPT-4o-mini / DeepSeek），BGE 中文 Embedding

## 技术栈

| 组件 | 技术 |
|------|------|
| LLM 框架 | LangChain + LangGraph |
| 向量数据库 | Milvus 2.x（混合检索：dense HNSW + sparse BM25） |
| Embedding | BAAI/bge-small-zh-v1.5（HuggingFace） |
| 大语言模型 | GPT-4o-mini / DeepSeek（OpenAI 兼容协议） |
| Web 搜索 | Tavily Search API |
| 文档解析 | 自定义 Markdown 解析器（按标题层级切分） |

## 项目结构

```
RAG_PROJECT/
├── agent/                  # LangChain Tool-Calling Agent
│   └── rag_agent.py        # 对话式 Agent，带多轮记忆
├── graph/                  # 基础 RAG 图（版本一）
│   ├── graph1.py           # 简单 RAG 工作流：检索 → 生成
│   ├── agent_node.py       # Agent 节点
│   ├── generate_node.py   # 回答生成节点
│   ├── rewrite_node.py     # 查询重写节点
│   └── graph_state1.py     # 图状态定义
├── graph2/                 # 自适应 RAG 图（版本二，核心）
│   ├── graph_2.py          # 完整 Adaptive RAG 工作流
│   ├── graph_state2.py     # 图状态定义
│   ├── retriever_node.py   # 文档检索节点
│   ├── grade_documents_node.py  # 文档相关性评分节点
│   ├── generate_node2.py   # 回答生成节点
│   ├── transform_query_node.py  # 查询重写节点
│   ├── web_search_node.py  # Web 搜索节点
│   ├── query_route_chain.py    # 问题路由（向量库 vs Web 搜索）
│   ├── grade_hallucinations_chain.py  # 幻觉检测
│   └── grade_answer_chain.py   # 答案质量评估
├── documents/              # 文档处理与向量库写入
│   ├── markdown_parser.py  # Markdown 结构化解析
│   ├── milvus_db.py        # Milvus Collection 创建与数据写入
│   └── write_milvus.py     # 批量写入脚本
├── llm_models/             # 模型配置
│   ├── all_llm.py          # LLM 初始化（GPT-4o-mini / DeepSeek）
│   └── embeddings_model.py  # Embedding 模型（BGE 中文）
├── tools/                  # 自定义工具
│   └── retriever_tools.py  # 检索工具封装
├── search_tool/            # Web 搜索工具
│   └── test_search.py      # Tavily 搜索测试
├── utils/                  # 工具函数
│   ├── env_utils.py        # 环境变量加载
│   ├── log_utils.py        # 日志配置
│   └── print_utils.py      # 打印工具
├── datas/                  # 示例数据
│   ├── layout-parser-paper.pdf  # 示例 PDF 文档
│   ├── md/                 # 示例 Markdown 文档
│   └── output/             # 解析后的 JSON 数据
├── test_load/              # 文档加载测试
├── test_milvus/            # Milvus 基础操作测试
├── test_vector/            # 向量检索测试
├── draw_png.py             # 工作流图可视化工具
├── graph_rag1.png          # 基础 RAG 流程图
├── graph_rag2.png          # 自适应 RAG 流程图
└── langchain_study/         # LangChain 系统学习（见子目录 README）
    ├── models/             # 模型调用、工具调用、结构化输出
    ├── agent_part/         # Agent 创建、流式、异步、错误处理
    ├── short_memory/       # 短期记忆与上下文管理
    ├── long_memory/        # 长期记忆
    ├── human_in_the_loop/  # 人机协同（HITL）
    ├── guardrails/          # 安全护栏（PII 脱敏等）
    ├── mcp_part/            # MCP 协议实战
    └── runtime_and_context_engineering/  # 运行时与上下文工程
```

## 自适应 RAG 工作流

```
用户问题
    │
    ▼
┌─────────────┐
│  问题路由    │ ◄── 判断走向向量库还是 Web 搜索
└──────┬──────┘
       │
   ┌───┴───┐
   ▼       ▼
向量检索  Web搜索
   │       │
   ▼       │
文档评分   │
   │       │
   ├── 有相关文档 ──→ 生成回答
   │       │
   └── 无相关文档
           │
           ▼
      查询重写（最多2次）
           │
           ├── 仍无文档 ──→ Web 搜索
           │
           ▼
      生成回答
           │
           ▼
    幻觉检测 + 答案评估
           │
     ┌─────┼─────┐
     ▼     ▼     ▼
   有用   无用   不支持
     │     │     │
    结束  重写   重新生成
```

## 快速开始

### 环境要求

- Python 3.11+
- 可访问的 Milvus 实例（本地或远程）

### 安装依赖

```powershell
# 创建虚拟环境（可选）
python -m venv .venv
.venv\Scripts\Activate.ps1

# 安装依赖
pip install langchain langgraph langchain-openai langchain-milvus pymilvus langchain-huggingface langchain-community python-dotenv loguru
```

### 配置环境变量

在项目根目录创建 `.env` 文件：

```env
# OpenAI 兼容 API（GPT-4o-mini 或中转）
OPENAI_API_KEY=sk-your-openai-api-key

# DeepSeek（可选，替代 OpenAI）
DEEPSEEK_API_KEY=sk-your-deepseek-api-key

# Milvus 连接地址（在 utils/env_utils.py 中也可修改）
# MILVUS_URI=http://your-milvus-host:19530
```

### 运行

```powershell
# 基础 RAG（简单检索 → 生成）
python graph/graph1.py

# 自适应 RAG（推荐，完整工作流）
python graph2/graph_2.py

# 对话式 Agent
python agent/rag_agent.py
```

## 核心模块说明

### 1. 基础 RAG（`graph/`）
最简单的 RAG 流程：用户问题 → 向量检索 → 重排/过滤 → LLM 生成回答。适合理解 RAG 基本原理。

### 2. 自适应 RAG（`graph2/`）
生产级 RAG 工作流，包含完整的质量保障闭环：
- **问题路由**：LLM 判断问题适合查向量库还是直接 Web 搜索
- **文档评分**：检索后逐篇评估文档相关性，过滤不相关内容
- **查询重写**：无相关文档时自动改写查询重新检索（最多 2 次）
- **幻觉检测**：验证生成回答是否基于检索文档
- **答案评估**：检查回答是否正确回应用户问题

### 3. Milvus 混合检索（`documents/milvus_db.py`）
- 稠密向量：BGE-small-zh-v1.5（512 维），HNSW 索引
- 稀疏向量：BM25 内置函数，中文 jieba 分词
- 混合检索同时利用语义相似性和关键词匹配

## 作者

[guui913](https://github.com/guui913) — AI 大模型应用开发学习实践
