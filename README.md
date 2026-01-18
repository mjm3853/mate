# mate

General purpose workflow automation tools built around self-hosted n8n with LLM integrations.

## Quick Start

```bash
cp .env.example .env
# Edit .env with your API keys

docker compose up -d
```

Access n8n at http://localhost:5678

## Services

- **n8n** (5678): Workflow automation with AI nodes
- **n8n-mcp** (3000): MCP server for AI assistant integration
- **PostgreSQL**: Persistent storage

## MCP Setup (Claude Desktop)

Copy `config/claude_desktop_config.example.json` to your Claude config directory and update the API key.
