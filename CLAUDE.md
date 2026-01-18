# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Mate is a workflow automation toolkit built around self-hosted n8n with LLM integrations.

## Development Commands

### Docker Services

```bash
# Start all services (n8n, PostgreSQL)
docker compose up -d

# Stop all services
docker compose down

# View logs
docker compose logs -f n8n

# Restart n8n after config changes
docker compose restart n8n
```

### First-Time Setup

```bash
cp .env.example .env
# Edit .env with API keys

docker compose up -d
# Access n8n at http://localhost:5678
# Create account, generate API key from Settings > API, add to .env as N8N_API_KEY
```

### Workflow Management

```bash
# Export a workflow from n8n (replace WORKFLOW_ID)
curl -H "X-N8N-API-KEY: $N8N_API_KEY" http://localhost:5678/api/v1/workflows/WORKFLOW_ID > n8n/workflows/workflow-name.json

# Import a workflow to n8n
curl -X POST -H "X-N8N-API-KEY: $N8N_API_KEY" -H "Content-Type: application/json" \
  -d @n8n/workflows/workflow-name.json http://localhost:5678/api/v1/workflows
```

### Python Utilities

```bash
uv sync
uv run python hello.py
uv run pytest
```

## Architecture

### Services

- **n8n** (port 5678): Workflow automation engine with AI nodes enabled (`N8N_AI_ENABLED=true`)
- **PostgreSQL**: Persistent storage for workflows and execution history

### MCP Integration

Use n8n's built-in MCP Server Trigger node to expose workflows as MCP tools. Configure within the n8n UI.

## Workflow Patterns

Exported workflows in `n8n/workflows/` demonstrate:

- **sample-llm-webhook.json**: Webhook → AI Agent → Response pattern using LangChain nodes
- **topic-scanner-daily.json**: Scheduled task → multi-source search (Reddit + SerpAPI) → LLM filtering → email digest
