# Streamlit Native MCP & Agent API Example

Minimal example demonstrating Streamlit 1.67.0's `server.enableAgentApi` setting, exposing a standard Streamlit app simultaneously as a human UI, a Model Context Protocol (MCP) server, and a headless HTTP endpoint.

## Quickstart

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
streamlit run app.py
```

The app starts on `http://127.0.0.1:8501` with `.streamlit/config.toml` enabling `enableAgentApi = true`.

## Connect an MCP Client

Point any Streamable HTTP MCP client to `http://127.0.0.1:8501/_stcore/agent/mcp`:

**Claude Code CLI:**
```bash
claude mcp add --transport http unit-economics http://127.0.0.1:8501/_stcore/agent/mcp
```

**Cursor (`.cursor/mcp.json`):**
```json
{
  "mcpServers": {
    "unit-economics": {
      "url": "http://127.0.0.1:8501/_stcore/agent/mcp"
    }
  }
}
```

## Test Directly Over HTTP

1. Initialize a session:
```bash
curl -s -X POST http://127.0.0.1:8501/_stcore/agent/interact \
  -H "Content-Type: application/json" \
  -d '{}'
```

2. Send widget inputs using the returned `session_id`:
```bash
curl -s -X POST http://127.0.0.1:8501/_stcore/agent/interact \
  -H "Content-Type: application/json" \
  -d '{
    "session_id": "PASTE_SESSION_ID_HERE",
    "widget_state": {
      "accounts": 600,
      "arpa": 450,
      "churn_pct": 1.5
    }
  }'
```
