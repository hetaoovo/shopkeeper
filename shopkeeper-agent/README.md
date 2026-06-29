<div align="center">
  <h1>电商智能问数 Agent</h1>
  <h4><b>shopkeeper-agent</b></h4>
  <p>
    基于大语言模型、混合检索与多阶段工作流编排的电商数据分析系统。
    支持自然语言提问、元数据召回、SQL 生成校验、查询执行与流式结果返回。
  </p>
</div>

<div align="center">

![AI](https://img.shields.io/badge/AI-Agent-00c853?style=flat)
![Python](https://img.shields.io/badge/Python-3.14-3776AB.svg?logo=python&logoColor=white)
![LangGraph](https://img.shields.io/badge/LangGraph-Agentic%20Workflow-1C3C3C.svg)
![FastAPI](https://img.shields.io/badge/FastAPI-Backend-009688.svg)
![React](https://img.shields.io/badge/React-Frontend-61DAFB.svg?logo=react&logoColor=black)

</div>

## 项目简介

`shopkeeper-agent` 是一个面向电商数据分析场景的智能问数系统。

在传统数据分析流程中，业务人员通常需要依赖数据分析师编写 SQL，才能查询销售额、订单量、地区分布、商品表现等指标。本项目通过自然语言交互的方式，让用户直接提出业务问题，系统自动完成元数据召回、SQL 生成、SQL 校验、查询执行和结果返回。

项目核心目标是构建一条完整的智能问数链路：

- 用户使用自然语言提出分析问题
- 系统检索相关数据表、字段、指标和字段取值
- 大模型结合上下文生成 SQL
- 系统对 SQL 进行校验、修正和执行
- 前端以流式方式展示执行过程和查询结果

相比直接让大模型生成 SQL，本项目引入了元数据知识库、向量检索、全文检索和多阶段 Agent 工作流，使 SQL 生成过程更可控，也更接近真实业务系统中的智能数据分析流程。

## 功能特性

- **自然语言问数**
  - 支持用户通过自然语言描述数据分析需求。
  - 自动理解问题中的指标、维度、筛选条件和时间范围。

- **元数据知识库**
  - 管理表结构、字段说明、业务指标和字段取值。
  - 为后续检索、上下文构建和 SQL 生成提供基础数据。

- **混合检索能力**
  - 使用向量检索召回语义相关的字段和指标。
  - 使用全文检索召回字段真实取值。
  - 使用 MySQL 保存权威结构化元数据。

- **多阶段 Agent 工作流**
  - 基于 LangGraph 编排问数流程。
  - 将关键词抽取、信息召回、上下文构建、SQL 生成、SQL 校验和 SQL 执行拆分为多个节点。
  - 便于调试、扩展和观测。

- **SQL 生成与执行闭环**
  - 自动生成 SQL。
  - 对 SQL 进行校验和错误修正。
  - 查询数据仓库并返回结构化结果。

- **流式响应**
  - 后端通过 SSE 返回执行进度、查询结果和异常信息。
  - 前端可以实时展示 Agent 的处理过程。

- **前后端完整实现**
  - 后端基于 FastAPI 提供接口服务。
  - 前端基于 React、Vite 和 Tailwind CSS 构建交互页面。

## 系统架构

![系统架构图](docs/images/shopkeeper-agent-system-architecture.svg)

系统主要由以下几个部分组成：

| 模块 | 说明 |
| --- | --- |
| 前端应用 | 提供自然语言问数入口、执行过程展示和结果展示 |
| FastAPI 服务 | 提供查询接口、依赖注入、生命周期管理和 SSE 流式响应 |
| LangGraph Agent | 编排问数流程，包括召回、推理、SQL 生成、校验和执行 |
| MySQL 数仓 | 存储模拟电商业务数据，用于最终 SQL 查询 |
| MySQL 元数据库 | 存储表、字段、指标、字段关系等结构化元数据 |
| Qdrant | 存储字段和指标向量，支持语义检索 |
| Elasticsearch | 存储字段真实取值，支持关键词和值域检索 |
| Embedding 服务 | 将字段、指标和问题文本转换为向量 |
| LLM 服务 | 负责关键词理解、上下文推理和 SQL 生成 |

## 技术栈

| 类型 | 技术 |
| --- | --- |
| 后端框架 | FastAPI |
| Agent 编排 | LangGraph |
| LLM 接入 | LangChain |
| 数据库 | MySQL |
| ORM | SQLAlchemy |
| 向量数据库 | Qdrant |
| 全文检索 | Elasticsearch |
| Embedding | TEI / BAAI/bge-large-zh-v1.5 |
| 前端框架 | React |
| 构建工具 | Vite |
| 样式方案 | Tailwind CSS |
| 流式协议 | Server-Sent Events |
| Python 依赖管理 | uv |
| 前端依赖管理 | pnpm |
| 容器化 | Docker Compose |


系统会自动完成相关字段和指标召回，并生成对应 SQL 查询结果。

## 核心流程

一次完整的问数请求大致包含以下步骤：

1. 接收用户自然语言问题
2. 抽取问题中的关键词和业务意图
3. 召回相关字段、指标和字段取值
4. 合并召回结果并构建上下文
5. 补充日期、表结构和数据库约束信息
6. 调用大模型生成 SQL
7. 对 SQL 进行校验
8. SQL 执行失败时进行修正
9. 查询数据仓库
10. 通过 SSE 返回执行过程和最终结果

## 应用场景

该项目适用于以下场景：

- 电商数据分析助手
- 企业内部自然语言 BI 查询
- 数据仓库智能查询入口
- LLM + 数据库结合的工程实践
- Agent 工作流编排实验
- Text-to-SQL 系统原型验证

## 后续规划

后续可以继续扩展以下能力：

- 用户登录与权限控制
- 数据权限和行列级权限管理
- SQL 白名单和安全审计
- 查询结果可视化
- 多轮问数和上下文记忆
- 查询缓存与性能优化
- Agent 执行链路观测
- 自动化评测集和回归测试
- 多数据源接入
- 生产环境部署方案

## 注意事项

- 本项目默认使用本地 Docker Compose 启动 MySQL、Qdrant、Elasticsearch 和 Embedding 服务。
- 首次运行前需要先下载 Embedding 模型。
- 大模型 API Key 需要自行配置。
- 项目中的数据主要用于本地开发、演示和功能验证，不建议直接用于生产环境。
- 如需在生产环境使用，需要额外补充权限、安全、审计、监控和稳定性治理能力。

## 致谢

本项目参考了公开课程和开源项目中的智能问数设计思路，并在此基础上进行了工程化整理和功能扩展。
