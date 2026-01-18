# mate

General purpose workflow automation tools built around self-hosted n8n with LLM integrations.

## Quick Start

```bash
# 1. Set up environment
cp .env.example .env
# Edit .env with your API keys

# 2. Install direnv (for auto-loading env vars)
brew install direnv
echo 'eval "$(direnv hook zsh)"' >> ~/.zshrc  # or ~/.bashrc for bash
source ~/.zshrc

# 3. Allow direnv in this directory
direnv allow

# 4. Start services
docker compose up -d
```

Access n8n at http://localhost:5678

After creating your n8n account, generate an API key from Settings > API and add it to `.env` as `N8N_API_KEY`.

## Services

- **n8n** (5678): Workflow automation with AI nodes
- **PostgreSQL**: Persistent storage

## MCP Integration

Use n8n's built-in MCP Server Trigger node to expose workflows as MCP tools. Configure within the n8n UI.
