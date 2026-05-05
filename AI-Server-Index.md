---
tags: [hardware, LLM, local-inference, AGI-infrastructure, MFG, semiconductor, Qwen, NVIDIA]
created: 2026-04-23
---

# 本地 AGI 推理伺服器 — 索引

> Dell Pro Max Tower T2 + RTX PRO 6000 96GB · Qwen3.6-35B-A3B FP8

## 文件清單

| 文件 | 說明 |
|------|------|
| [[On-premise-AGI-Server-Research]] | 硬體選型研究筆記（原始研究） |
| [[部署手冊]] | Step-by-step 安裝指南（給管理員） |
| [[工程師使用指南]] | 使用說明（給工廠工程師） |
| [[docker-compose.yml]] | 完整生產配置（直接使用） |
| [[.env.example]] | 環境變數範本 |
| [[nginx-dify.conf]] | Nginx 反向代理設定 |

## 架構速覽

```
LAYER 1 — 推理引擎
  vLLM :8000        ← 生產推理（Qwen3.6-35B-A3B FP8）
  Ollama :11434     ← 開發測試

LAYER 2 — 介面 & 編排
  Open WebUI :8080  ← 工程師聊天入口
  Dify :3000        ← Agent 建構平台
  n8n :5678         ← 自動化工作流程

LAYER 3 — 記憶 & 知識
  Qdrant :6333      ← 向量資料庫
  Mem0 :8888        ← 長期跨 session 記憶
```

## 快速指令

```bash
# 啟動所有服務
docker compose up -d

# 查看狀態
docker compose ps

# 查看 GPU 使用率
watch -n 2 nvidia-smi

# 停止服務
docker compose down
```

## 採購狀態

- [ ] 向 Dell 台灣業務索取報價
- [ ] 確認 1500W Platinum PSU
- [ ] 確認 3 年 ProSupport Plus
- [ ] 購買 UPS 不斷電系統
- [ ] HuggingFace Token 申請
