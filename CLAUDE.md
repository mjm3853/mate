# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Mate is a workflow automation toolkit built around self-hosted n8n with LLM integrations and MCP (Model Context Protocol) server support for AI assistant interactions.

## Project Structure

```
mate/
├── docker-compose.yml      # n8n, n8n-mcp, and PostgreSQL services
├── .env.example            # Environment variables template
├── config/
│   └── claude_desktop_config.example.json  # MCP config for Claude Desktop
├── n8n/
│   ├── data/              # n8n persistent data (gitignored)
│   ├── workflows/         # Exported workflow JSON files
│   └── backup/            # Workflow backups (gitignored)
└── hello.py               # Python utilities (future)
```

## Development Commands

### n8n Services

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
# Copy and configure environment
cp .env.example .env
# Edit .env with your API keys and tokens

# Start services
docker compose up -d

# Access n8n at http://localhost:5678
# Create account and generate API key from Settings > API
# Add API key to .env as N8N_API_KEY
```

### MCP Server Access

The n8n-mcp service runs at `http://localhost:3000` and provides:
- HTTP Streamable endpoint: `http://localhost:3000/mcp`
- Access to 1,084 n8n node documentation for AI assistants
- Workflow management when N8N_API_KEY is configured

### Claude Desktop Integration

Copy config from `config/claude_desktop_config.example.json` to:
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`
- Linux: `~/.config/Claude/claude_desktop_config.json`

### Python Utilities

```bash
uv sync
uv run python hello.py
uv run pytest
```

## Architecture

### Services

- **n8n** (port 5678): Workflow automation engine with AI nodes enabled
- **n8n-mcp** (port 3000): MCP server providing n8n documentation to AI assistants
- **PostgreSQL**: Persistent storage for workflows and execution history

### n8n MCP Integration

Two approaches for AI assistant integration:

1. **n8n-mcp server** (this setup): Gives AI assistants knowledge about n8n nodes and can manage workflows via API
2. **n8n's built-in MCP Server Trigger**: Exposes specific workflows as MCP tools (configure within n8n UI)

### LLM Integrations

Configure API keys in `.env` for n8n's AI nodes:
- OpenAI (GPT-4, etc.)
- Anthropic (Claude)
- Add others as needed (Google AI, Azure OpenAI, HuggingFace)

## Key Environment Variables

| Variable | Purpose |
|----------|---------|
| `N8N_API_KEY` | n8n API access for MCP workflow management |
| `MCP_AUTH_TOKEN` | Bearer token for MCP HTTP authentication |
| `OPENAI_API_KEY` | OpenAI API for n8n AI nodes |
| `ANTHROPIC_API_KEY` | Anthropic API for n8n AI nodes |
| `POSTGRES_PASSWORD` | Database password (change in production) |

## Requirements

- Docker and Docker Compose
- Python 3.12+ (for local utilities)
