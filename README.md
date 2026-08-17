# 📚 智能文档问答助手

基于 [HelloAgents](https://github.com/helloagents) 的 PDF 学习助手，支持加载 PDF 文档、基于 RAG 的智能问答、学习历程记录与学习报告生成。

## ✨ 功能特性

- 📄 **加载 PDF 文档**：上传 PDF，自动解析、分块、向量化并构建知识库
- 💬 **智能问答**：基于 RAG（检索增强生成），从文档中检索相关内容并生成准确回答
- 📝 **学习笔记**：记录学习心得与重要概念
- 🧠 **学习回顾**：回顾历史学习内容，基于记忆系统
- 📊 **学习统计 & 报告**：查看学习进度，生成学习报告

## 🏗️ 技术架构

| 组件 | 技术 | 作用 |
|---|---|---|
| Agent 框架 | HelloAgents（RAGTool + MemoryTool） | RAG 检索 + 记忆管理 |
| 大模型 LLM | 阿里云百炼 DashScope（qwen3.7-plus） | 生成回答 |
| 嵌入模型 | DashScope（text-embedding-v4） | 文本向量化 |
| 向量数据库 | Qdrant Cloud | 存储文档向量，语义检索 |
| 图数据库 | Neo4j AuraDB | 存储实体关系记忆 |
| Web 界面 | Gradio | 图形化交互界面 |

## 📋 环境要求

- Python 3.11
- 以下三个云服务的账号与密钥：
  1. **阿里云百炼（DashScope）**：LLM + 嵌入模型 API Key
  2. **Qdrant Cloud**：向量数据库实例
  3. **Neo4j AuraDB**：图数据库实例

## 🚀 快速开始

### 1. 创建虚拟环境

```bash
conda create -n hello_agent_env python=3.11 -y
conda activate hello_agent_env
```

### 2. 安装依赖

```bash
pip install -r requirements.txt
```

### 3. 配置环境变量

复制示例配置，填入你自己的密钥：

```bash
copy .env.example .env   # Windows
# 或
cp .env.example .env     # macOS / Linux
```

然后编辑 `.env`，填入你的 DashScope、Qdrant、Neo4j 连接信息。

### 4. 运行

```bash
python 11_Q&A_Assistant.py
```

浏览器访问 **http://127.0.0.1:7860/**

## ⚙️ 配置说明（`.env`）

```ini
# ===== LLM 大模型配置 =====
LLM_API_KEY=你的-阿里云百炼-API-Key
LLM_MODEL_ID=qwen3.7-plus
LLM_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1

# ===== Qdrant 向量数据库 =====
QDRANT_URL=https://你的实例ID.us-west-1-0.aws.cloud.qdrant.io:6333
QDRANT_API_KEY=你的-Qdrant-API-Key

# ===== Neo4j 图数据库 =====
NEO4J_URI=neo4j+s://你的实例ID.databases.neo4j.io
NEO4J_USERNAME=neo4j          # AuraDB 默认用户名是 neo4j，不是实例 ID
NEO4J_PASSWORD=你的-Neo4j-密码
NEO4J_DATABASE=neo4j          # AuraDB 默认数据库名是 neo4j

# ===== 嵌入模型 =====
EMBED_MODEL_TYPE=dashscope
EMBED_MODEL_NAME=text-embedding-v4
EMBED_API_KEY=你的-阿里云百炼-API-Key
EMBED_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
```

## 📖 使用说明

1. **初始化助手**：在「开始使用」标签页输入用户 ID（决定知识库分区，建议固定一个），点击「初始化助手」
2. **加载文档**：上传 PDF，点击「加载文档」
3. **智能问答**：在「智能问答」标签页输入问题，如"什么是 Transformer？"
4. **回顾历史**：输入"我之前学过什么？"等关键词，触发学习回顾
5. **学习笔记**：在「学习笔记」标签页记录心得
6. **统计报告**：在「学习统计」标签页查看进度、生成报告

> 💡 **记忆持久化**：文档向量存 Qdrant 云端，记忆存 Neo4j 云端，重启程序后**用相同的用户 ID** 即可继续访问之前的数据。

## ⚠️ 注意事项 / 已知问题

1. **qdrant-client 必须固定 1.15.x**：1.19+ 移除了 `SearchRequest`，会导致 RAG 初始化失败
2. **pdfplumber 是 PDF 必需的**：`hello-agents[all]` 默认装的是 `markitdown`（基础版），不含 PDF 解析依赖，必须手动装 `pdfplumber`
3. **hello-agents 必须固定 0.2.0**：1.0.0 移除了 `RAGTool`/`MemoryTool`，API 不兼容
4. **云服务会到期**：Qdrant / Neo4j 免费实例可能因不活跃被回收，若发现"数据突然搜不到"，先检查云端实例是否存活


## 📄 许可证

仅供学习交流使用。
