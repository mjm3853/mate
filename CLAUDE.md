# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Mate is a workflow automation toolkit built around self-hosted n8n with LLM integrations and MCP (Model Context Protocol) server support for AI assistant interactions.

## Development Commands

### Docker Services

```bash
# Start all services (n8n, n8n-mcp, PostgreSQL)
docker compose up -d

# Stop all services
docker compose down

# View logs
docker compose logs -f n8n
docker compose logs -f n8n-mcp

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
- **n8n-mcp** (port 3000): MCP server providing n8n node documentation to AI assistants
- **PostgreSQL**: Persistent storage for workflows and execution history

### MCP Integration

Two approaches for AI assistant integration:

1. **n8n-mcp server** (this setup): HTTP endpoint at `http://localhost:3000/mcp` - gives AI assistants knowledge about n8n's 1,084 nodes and can manage workflows via API
2. **n8n's built-in MCP Server Trigger**: Exposes specific workflows as MCP tools (configure within n8n UI)

### Claude Desktop Integration

Copy `config/claude_desktop_config.example.json` to:
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`
- Linux: `~/.config/Claude/claude_desktop_config.json`

## Workflow Patterns

Exported workflows in `n8n/workflows/` demonstrate:

- **sample-llm-webhook.json**: Webhook → AI Agent → Response pattern using LangChain nodes
- **topic-scanner-daily.json**: Scheduled task → multi-source search (Reddit + SerpAPI) → LLM filtering → email digest
