# 个人 AI 项目集

五个端到端 AI 应用的统一入口。每个项目都完成了「需求 → 编码 → 容器化 → 上线部署」的完整闭环，源码开源在 GitHub，线上实例长期运行、可直接访问。

## 线上地址

| 入口 | 地址 |
|------|------|
| **统一门户** | https://qiaokeshen.asia/ |
| P1 知识库 RAG | https://rag.qiaokeshen.asia/ |
| P2 多 Agent 助手 | https://sc.qiaokeshen.asia/ |
| P4 合同智能审核 | https://cr.qiaokeshen.asia/ |
| P5 车牌识别 | https://lpr.qiaokeshen.asia/ |
| P6 智能体商城 | https://mall.qiaokeshen.asia/ |

## 项目矩阵

| 项目 | 能力维度 | 技术栈 | 核心指标 |
|------|---------|--------|---------|
| P1 企业知识库 RAG | 数据底座（知识注入） | Flask + Milvus + BM25/向量混合检索 + RRF + bge-reranker + SSE | 召回 62%→89%，1.6 万 chunks |
| P2 供应链多 Agent 助手 | 业务应用层（Agent 落地） | FastAPI + LangGraph + Pydantic + 微服务 | 完成率 85%→94%，15min→30s |
| P4 采购合同审核 | 垂直场景（行业应用） | FastAPI + OCR + LLM 结构化抽取 + YAML 规则引擎 | F1 95.2%，40min→3min |
| P5 车牌识别 · 实时视频流 | 实时视觉（视频流理解） | YOLOv8s + LPRNet + hyperlpr3(ONNX) + OpenCV + FastAPI + MJPEG | 单次识别 491→130ms，169 单测全绿（CPU 实测） |
| P6 智能体商城 · AgentMall | 业务系统（交易闭环 + 智能体编排） | Spring Boot 3.3 + PostgreSQL 16 + TCC/Saga/Outbox + FastAPI/SSE + Docker | 7/7 部署单元可运行，160 项端到端断言全通过，同键 100 并发仅 1 次副作用 |

## 技术说明

- 纯静态 HTML（零依赖、零构建），nginx 静态 serve。
- 五个子域项目均通过 nginx 反代 + Let's Encrypt 证书实现 HTTPS。
- 线上入口采用「新标签页打开」规避 iframe 跨域/混合内容问题；页面内嵌架构图与量化指标卡作离线兜底。
- **P5 部署环境**：纯 CPU（`gpu:false`），性能指标为实测值。自研级联权重待训练，精度类指标（车辆 mAP / 整牌准确率）待自有模型产出后补充。
- **P6 部署形态**：7 个部署单元（5 个 Java 服务 + 嵌入式 PostgreSQL 16 + Python 编排层）合并为**单容器**运行，容器内全部服务仅绑 `127.0.0.1`，只由宿主机 nginx 反代 `8080` 对外——`8081~8084 / 8090 / 5432` 公网不可达。

## CI/CD 部署

push 到 `main` 分支自动触发 GitHub Actions：

```
push → appleboy/ssh-action → 腾讯云 git pull → nginx 静态 serve → 健康检查
```

Secrets 需配置：`SSH_HOST`、`SSH_USER`、`SSH_PRIVATE_KEY`。
