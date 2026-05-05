# SageVego — 本地 AGI 推理伺服器 · 封測廠部署包

> Dell Pro Max Tower T2 + NVIDIA RTX PRO 6000 96GB · Qwen3.6-35B-A3B FP8

## 📦 檔案說明

| 檔案 | 說明 |
|------|------|
| `manufacturing-AGI-portal.html` | 製造業 AGI 入口網站（主要交付物）|
| `manufacturing_llm_applications.html` | 製造業 LLM 應用地圖 |
| `docker-compose.yml` | 完整生產 Docker Compose 配置 |
| `.env.example` | 環境變數範本（複製為 `.env` 後填入實際值）|
| `nginx-dify.conf` | Nginx 反向代理設定（Dify）|
| `部署手冊.md` | Step-by-step 安裝指南（給管理員）|
| `工程師使用指南.md` | AGI 助手使用說明（給工廠工程師）|
| `AGI-Server-Index.md` | 系統架構索引 |

## 🏗️ 架構概覽

\`\`\`
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
\`\`\`

## 🚀 快速啟動

\`\`\`bash
cp .env.example .env
# 編輯 .env，填入 SERVER_IP、HF_TOKEN 等變數
docker compose up -d
\`\`\`

## 🔗 Manufacturing AGI Portal

開啟 `manufacturing-AGI-portal.html` 可直接在瀏覽器使用，或部署至內網 Web Server。

功能包含：
- 角色選擇（製程 / 設備 / 品質 / 管理員）
- 6 種 AGI 助手（製程、RCA、良率、設備、文獻、程式碼）
- 9-Agent RCA Pipeline 監控
- 良率趨勢自動報告
- 知識庫管理
