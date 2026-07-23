# GEOly MCP Server Installation Guide

This guide is written for AI agents (Cline, Claude, Cursor agents, etc.) installing the GEOly MCP server on a user's behalf. Follow the steps for the client you are running in.

GEOly is a **hosted remote MCP server**. There is no package to download, no process to run, and no API key to paste — the entire installation is: add one URL to the client's MCP configuration, then let the user complete an OAuth sign-in in their browser.

Server URL (streamable HTTP):

```
https://app.geoly.ai/api/mcp
```

## Prerequisites

- A GEOly account with at least one workspace ([www.geoly.ai](https://www.geoly.ai)). For meaningful query results the workspace should have a monitored brand with data; a brand-new workspace connects fine but returns empty datasets.
- A browser available for the OAuth consent (interactive use), **or** a `geom_...` static token generated in the user's GEOly workspace settings (headless/CI use, always read-only).

## Installation steps

### 1. Add the server to the client's MCP configuration

Pick the block that matches the client, using the exact file paths given.

**Claude Code** — run:

```bash
claude mcp add --transport http geoly https://app.geoly.ai/api/mcp
```

**Cursor** — merge into `~/.cursor/mcp.json` (create the file if missing):

```json
{
  "mcpServers": {
    "geoly": {
      "url": "https://app.geoly.ai/api/mcp"
    }
  }
}
```

**Cline** — Cline supports remote servers natively. Merge into the MCP settings file (note the camel-cased `streamableHttp`; omitting `type` falls back to legacy SSE and fails):

```json
{
  "mcpServers": {
    "geoly": {
      "type": "streamableHttp",
      "url": "https://app.geoly.ai/api/mcp"
    }
  }
}
```

Settings file locations:
- VS Code extension: reachable via MCP Servers → Configure MCP Servers, or directly:
  - macOS: `~/Library/Application Support/Code/User/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json`
  - Windows: `%APPDATA%\Code\User\globalStorage\saoudrizwan.claude-dev\settings\cline_mcp_settings.json`
  - Linux: `~/.config/Code/User/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json`
- Cline CLI: `~/.cline/mcp.json`

If the OAuth browser flow does not trigger after adding the native config, fall back to the `mcp-remote` bridge (same JSON shape as the Claude Desktop file-based block below).

**Claude Desktop** — prefer the UI: Settings → Connectors → Add custom connector → URL `https://app.geoly.ai/api/mcp`. If only file access is available, merge this `mcp-remote` bridge block into `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "geoly": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://app.geoly.ai/api/mcp"]
    }
  }
}
```

- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`

**VS Code (Copilot)** — run:

```bash
code --add-mcp '{"name":"geoly","type":"http","url":"https://app.geoly.ai/api/mcp"}'
```

**Windsurf** — merge into `~/.codeium/windsurf/mcp_config.json` (note the `serverUrl` key):

```json
{
  "mcpServers": {
    "geoly": {
      "serverUrl": "https://app.geoly.ai/api/mcp"
    }
  }
}
```

**Codex CLI** — use the GEOly plugin marketplace (the plugin registers the remote server and handles OAuth; do not hand-write config):

```bash
codex plugin marketplace add geoly-ai/codex-plugins
codex plugin add geoly-mcp@geoly
```

**Headless / CI (any client)** — add an authorization header with a static token instead of OAuth. Example (Claude Code):

```bash
claude mcp add --transport http geoly https://app.geoly.ai/api/mcp \
  --header "Authorization: Bearer geom_YOUR_TOKEN"
```

### 2. Complete authentication

- Restart or reload the client so it picks up the new server.
- Trigger any GEOly tool call (see step 3). The server answers the first unauthenticated call with an HTTP 401 OAuth challenge — the client will open a browser window.
- Tell the user to sign in, choose which workspaces to share, and review the permission grid: read access is preselected; write access (4 tools) stays off unless they tick it.
- If no browser opens, the client does not support remote OAuth — switch to the `mcp-remote` bridge configuration shown above, or use a static token.

### 3. Verify installation

Run these calls in order:

1. `get_current_date` — must return the server time. **This is the pass/fail gate**: if it succeeds, connectivity, auth, and installation are all working.
2. `get_brand_overview` with `time_range` = `30d` — a data smoke test, not an installation gate:
   - KPIs returned (AIGVR / mention rate / citation rate) → everything works end to end.
   - Empty data or a "no brand" style error → installation is still **correct**; the workspace simply has no monitored brand with data yet. Tell the user to finish brand onboarding at app.geoly.ai.
   - A brand/workspace routing error on a multi-workspace connection → call `list_brands` (and `list_organizations` if present) first, or pin one workspace by using `https://app.geoly.ai/api/mcp?org_id=<id>` as the server URL.

Optional connectivity pre-check (distinguishes network problems from OAuth problems before any client is involved):

```bash
curl -s -o /dev/null -w "%{http_code}" -X POST https://app.geoly.ai/api/mcp \
  -H "content-type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{}}'
```

`401` means the endpoint is reachable and issuing its OAuth challenge correctly (expected without credentials). A timeout or 5xx is a network/service problem, not a configuration problem.

## Available tools (summary)

60+ read-mostly tools across: brand KPIs and daily trends, prompt-level visibility and AI answers, citation domains/pages, competitors and sentiment, GEO site audits, GA4, market intelligence (topic leaderboards, brand comparison, category whitespace, brand momentum, AI-search demand, shopping), public source domains, and 4 write tools (create prompt/topic/competitor, trigger monitoring). The tool surface adapts to plan and granted permissions — see the [README](./README.md#tools) for the full catalog.

## Troubleshooting

- **401 loop, browser never opens** — client lacks remote-OAuth support. Use the `mcp-remote` bridge or a `geom_` token.
- **402 Payment Required** — the workspace subscription is inactive; the user needs to check billing at app.geoly.ai.
- **Market-intelligence tool groups missing from the tool list** (topics, public brands, categories, search queries, shopping) — the workspace plan is below Grow. This is expected, not an installation failure. The two public source domain tools (`get_public_sources_overview`, `get_public_source_domain_detail`) are not plan-gated and should still be present.
- **Write tools missing** — write access wasn't ticked at consent, the connection uses a static token, or it spans multiple workspaces. Re-authenticate on a single workspace and grant the write permissions needed.
- **Stale or broken auth after a long idle period** — clear the client's cached credentials for `geoly` (e.g. `claude mcp remove geoly` then re-add, or the client's re-authenticate action) and redo step 2.
- **GET request to the URL returns 405** — expected; the endpoint only accepts MCP POST traffic.
