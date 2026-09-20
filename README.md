# AI 学习项目合集

个人 AI 大模型应用开发学习仓库，基于 LangChain / LangGraph 生态，涵盖 RAG 检索增强生成与 LangChain 核心特性两大模块。

## 仓库结构

```
AI-Java-/
├── RAG/                    # RAG 检索增强生成项目
│   ├── README.md           #   项目详细说明
│   ├── graph/              #   基础 RAG（检索 → 生成）
│   ├── graph2/             #   自适应 RAG（路由/评分/幻觉检测/重写）
│   ├── documents/          #   Markdown 解析 + Milvus 向量库写入
│   ├── agent/              #   对话式 Agent（带多轮记忆）
│   ├── llm_models/         #   模型与 Embedding 配置
│   ├── tools/              #   检索工具封装
│   ├── datas/              #   示例数据（PDF / Markdown / JSON）
│   └── ...
│
└── langchain_study/        # LangChain 系统学习笔记
    ├── README.md           #   项目详细说明
    ├── models/             #   模型调用、工具调用、结构化输出
    ├── agent_part/         #   Agent 创建、流式、异步、错误处理
    ├── short_memory/       #   短期记忆与上下文管理
    ├── long_memory/        #   长期记忆
    ├── human_in_the_loop/  #   人机协同（HITL）
    ├── guardrails/          #   安全护栏（PII 脱敏等）
    ├── mcp_part/            #   MCP 协议实战
    └── runtime_and_context_engineering/  # 运行时与上下文工程
```

## 项目说明

### RAG/ — 检索增强生成

基于 **LangGraph + Milvus** 的 RAG 实践，从基础检索到自适应 RAG 完整闭环：
- Milvus 混合检索（稠密 HNSW + 稀疏 BM25 中文分词）
- 自适应工作流：问题路由 → 文档评分 → 查询重写 → 幻觉检测 → 答案评估
- BGE 中文 Embedding + GPT-4o-mini / DeepSeek

详见 [RAG/README.md](RAG/README.md)

### langchain_study/ — LangChain 系统学习

系统学习 LangChain 的练习代码，覆盖：
- 模型调用（多供应商：DeepSeek / OpenAI / Claude / 通义 / 智谱 / Ollama）
- Agent 开发（工具调用、结构化输出、流式、异步、错误处理）
- 记忆系统（短期/长期、上下文裁剪、摘要）
- 人机协同（HITL：批准/拒绝/编辑/回复）
- 安全护栏（PII 脱敏、拦截、组合护栏）
- MCP 协议（快速开始、OAuth、拦截器、资源、通知、综合案例）

详见 [langchain_study/README.md](langchain_study/README.md)

## 技术栈

| 领域 | 技术 |
|------|------|
| LLM 框架 | LangChain + LangGraph |
| 向量数据库 | Milvus 2.x（混合检索） |
| Embedding | BAAI/bge-small-zh-v1.5 |
| 大语言模型 | DeepSeek / GPT-4o / Claude / 通义千问 / 智谱 GLM |
| MCP | Model Context Protocol |
| Web 搜索 | Tavily Search API |

## 作者

[guui913](https://github.com/guui913) — AI 大模型应用开发学习实践
