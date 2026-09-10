# AI 中台 · 统一门户

企业 AI 应用矩阵的统一演示入口，聚合三个已上线项目（P1/P2/P4），呈现「从 0 到 1 把 AI 能力做成产品」的故事线。

## 线上地址

| 入口 | 地址 |
|------|------|
| **统一门户** | https://qiaokeshen.asia/ |
| P1 知识库 RAG | https://rag.qiaokeshen.asia/ |
| P2 多 Agent 助手 | https://sc.qiaokeshen.asia/ |
| P4 合同智能审核 | https://cr.qiaokeshen.asia/ |

## 项目矩阵

| 项目 | 能力维度 | 技术栈 | 核心指标 |
|------|---------|--------|---------|
| P1 企业知识库 RAG | 数据底座（知识注入） | Flask + Milvus + BM25/向量混合检索 + RRF + bge-reranker + SSE | 召回 62%→89%，1.6 万 chunks |
| P2 供应链多 Agent 助手 | 业务应用层（Agent 落地） | FastAPI + LangGraph + Pydantic + 微服务 | 完成率 85%→94%，15min→30s |
| P4 采购合同审核 | 垂直场景（行业应用） | FastAPI + OCR + LLM 结构化抽取 + YAML 规则引擎 | F1 95.2%，40min→3min |

## 技术说明

- 纯静态 HTML（零依赖、零构建），nginx 静态 serve。
- 三个子域项目均通过 nginx 反代 + Let's Encrypt 证书实现 HTTPS。
- 线上演示采用「新标签页打开」规避 iframe 跨域/混合内容问题；页面内嵌架构图与量化指标卡作离线兜底。

## CI/CD 部署

push 到 `main` 分支自动触发 GitHub Actions：

```
push → appleboy/ssh-action → 腾讯云 git pull → nginx 静态 serve → 健康检查
```

Secrets 需配置：`SSH_HOST`、`SSH_USER`、`SSH_PRIVATE_KEY`。
