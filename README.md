# MFG AGI Dashboard

A web-based AI assistant portal for semiconductor assembly & test (OSAT) manufacturing engineers, powered by Claude API and deployed on Vercel.

**Live:** [mfg.sagevelo.com](https://mfg.sagevelo.com)

---

## Features

### Role-Based Access
Engineers select their role on entry, which customizes the interface and available tools:
- 製程工程師 (Process Engineer)
- 設備工程師 (Equipment Engineer)
- 品質工程師 (Quality Engineer)
- 管理員 (Admin)

### 6 AI Agents
Each agent is specialized for a different manufacturing workflow:

| Agent | Function |
|-------|----------|
| 製程工程師助手 | Process parameters, DOE recommendations, SOP Q&A |
| RCA 分析助手 | Root cause analysis with 9-agent pipeline |
| 良率報告助手 | Yield trend reports, WAT/CPK analysis, MRAT summaries |
| 設備維護助手 | Wire Bonder / Die Attach fault diagnosis, PM SOP |
| 技術文獻搜尋 | Technical literature search, JEDEC/AEC spec lookup |
| 程式碼助手 | Python data analysis, SQL queries, WAT box plot generation |

### Dashboard
- Real-time yield monitoring (Mobile / MCU A10 process lines)
- 9-Agent RCA pipeline queue and status
- Knowledge base document management
- Automated yield reports

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML/CSS/JavaScript |
| AI Backend | Claude API (`claude-3-5-sonnet-20241022`) |
| API Layer | Vercel Serverless Functions (`/api/chat.js`) |
| Hosting | Vercel |
| Domain | Squarespace DNS → `mfg.sagevelo.com` |

---

## Project Structure

```
/
├── index.html                    # Main portal (entry point)
├── manufacturing-ai-portal.html  # Source file (synced to index.html)
├── manufacturing_llm_applications.html  # LLM application map
├── api/
│   └── chat.js                   # Vercel serverless function → Claude API
├── docker-compose.yml            # Local inference server config (optional)
├── nginx-dify.conf               # Nginx reverse proxy config
├── .env.example                  # Environment variable template
├── 部署手冊.md                   # Deployment guide (admin)
└── 工程師使用指南.md             # User guide (engineers)
```

---

## Environment Variables

Set the following in Vercel Dashboard → Settings → Environment Variables:

| Variable | Description |
|----------|-------------|
| `CLAUDE_API_KEY` | Anthropic API key (`sk-ant-...`) |

---

## Local Development

1. Clone the repo
2. Open `index.html` directly in a browser for UI preview (chat won't work without API)
3. For full functionality, deploy to Vercel with `CLAUDE_API_KEY` set

---

## API Endpoint

`POST /api/chat`

**Request:**
```json
{
  "messages": [{ "role": "user", "content": "..." }],
  "systemPrompt": "You are a process engineer assistant..."
}
```

**Response:** Claude API message object

---

## Optional: Local Inference Server

For air-gapped factory deployments, `docker-compose.yml` provides a full local stack:

```
vLLM :8000       ← Production inference (Qwen3.6-35B-A3B FP8)
Open WebUI :8080 ← Engineer chat interface
Dify :3000       ← Agent builder
Qdrant :6333     ← Vector database
Mem0 :8888       ← Long-term memory
```

```bash
cp .env.example .env
# Fill in SERVER_IP, HF_TOKEN, etc.
docker compose up -d
```
