# LangChain 学习笔记

系统学习 LangChain 的练习代码，覆盖模型调用、Agent、记忆系统、Guardrails、HITL、MCP 等核心主题。

## 目录结构

```
langchain_study/
├── models/                    # 模型调用基础
│   ├── 01_quick_start.py      # init_chat_model 快速开始
│   ├── 02_invoke_llm.py       # 同步调用
│   ├── 03_invoke_llm2.py      # 模型配置
│   ├── 04_ainvoke_astream_abatch.py  # 异步调用
│   ├── model_tool_calling/    # 工具调用（单工具/多工具/Agent）
│   ├── model_structured_output/  # 结构化输出（Pydantic/TypedDict/JSON Schema）
│   └── model_other/           # 推理模型、限流、模型配置
├── agent_part/                # Agent 开发
│   ├── 01_create_agent_staticmodel.py   # 静态模型创建 Agent
│   ├── 02_create_agent_dynamicmodel.py  # 动态模型创建 Agent
│   ├── 03_agent_invoke.py     # Agent 调用
│   ├── 04_agent_prompt.py     # 提示词模板
│   ├── 05_agent_dynamic_prompt.py  # 动态提示词
│   ├── agent_stream_invoke/   # 流式调用与 Checkpointer
│   ├── agent_structured_output/  # Agent 结构化输出（7种方式）
│   ├── async_agent_invoke/    # 异步 Agent
│   ├── create_tool/           # 自定义工具
│   └── handler_tool_call_error/  # 工具调用错误处理
├── short_memory/              # 短期记忆
│   ├── 01-03                 # 内存/数据库记忆
│   ├── 04-07                 # 自定义状态、工具/中间件修改状态
│   ├── 08                    # Context 与 State
│   └── llm_context/          # 上下文裁剪、删除、摘要、自定义消息
├── long_memory/               # 长期记忆
│   ├── 01-03                 # 内存/数据库长期记忆
│   ├── 04                    # 工具修改长期记忆
│   └── 05                    # 短期+长期记忆结合
├── human_in_the_loop/         # 人机协同（HITL）
│   ├── 01                    # HITL 中间件基础
│   ├── 02                    # 批准/拒绝
│   ├── 03                    # 编辑
│   ├── 04                    # 回复
│   ├── 05                    # 多决策
│   ├── 06                    # 综合示例
│   ├── 07                    # 流式 HITL
│   └── 08                    # 自定义 HITL
├── guardrails/                # 安全护栏
│   ├── 01-04                 # PII 脱敏/掩码/哈希/拦截
│   ├── 05                    # HITL 护栏
│   ├── 06-07                 # Agent 前后自定义护栏
│   └── 08                    # 多护栏组合
├── mcp_part/                  # MCP（Model Context Protocol）
│   ├── 01_quick_start/        # MCP 快速开始
│   ├── 02_mcp_oauth/          # MCP OAuth 认证
│   ├── 03_interceptor/        # 拦截器（注入上下文/读写存储/更新状态）
│   ├── 04_handler_too_error/  # 工具错误处理与重试
│   ├── 05_resources_and_prompt/  # MCP 资源与提示模板
│   ├── 06_notification_and_logs/  # 通知与日志
│   ├── 07_elicitation/        # 用户信息收集
│   └── 08_comprehensive_example/  # 综合实战（售后客服 Agent）
├── runtime_and_context_engineering/  # 运行时与上下文工程
│   ├── 01-04                 # 运行时基础、工具内、内存中、执行器
│   └── 05-07                 # 上下文工程
├── env_utils.py               # 环境变量加载（多模型 API Key）
├── init_llm.py                # 多模型初始化（DeepSeek/OpenAI/Claude/通义/智谱/Ollama）
└── my_llm.py                  # 自定义 LLM 封装
```

## 支持的模型供应商

通过 `init_chat_model` 统一接入：
- **DeepSeek**（deepseek-v4-pro / v4-flash）
- **OpenAI**（GPT-4）
- **Anthropic**（Claude 3.5 Haiku）
- **通义千问**（qwen-plus）
- **智谱**（GLM-4）
- **Ollama**（本地模型）

## 快速开始

```env
# .env 配置（按需填写）
DEEPSEEK_API_KEY=sk-your-key
DEEPSEEK_BASE_URL=https://api.deepseek.com
OPENAI_API_KEY=sk-your-key
OPENAI_BASE_URL=https://api.openai.com/v1
```

```powershell
pip install langchain langchain-openai langchain-anthropic langchain-deepseek langchain-community python-dotenv
```
