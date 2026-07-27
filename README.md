English | [简体中文](./README.zh-CN.md)

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/geoly-ai/GEOly-MCP/main/assets/geoly-icon-dark.png">
  <img src="https://raw.githubusercontent.com/geoly-ai/GEOly-MCP/main/assets/geoly-icon.png" align="right" width="72" alt="GEOly logo">
</picture>

# GEOly MCP Server

The official remote MCP server for **[GEOly](https://www.geoly.ai)** — AI brand visibility (GEO) for your agent. GEOly tracks how brands are mentioned and cited across AI engines (ChatGPT, Perplexity, Google AI Mode, Google AI Overview, Gemini, Copilot), and this server puts that data — visibility KPIs, competitor share, citation sources, market intelligence, and site audits — directly into Claude, Cursor, Codex, VS Code, or any MCP client.

Hosted, streamable HTTP, OAuth in the browser. One URL, nothing to run locally:

```
https://app.geoly.ai/api/mcp
```

## What your agent can do

- **Pull the same KPIs you see in the app** — AIGVR score, mention rate, citation rate per AI platform (`get_brand_overview`), daily trends, and SQL-free controlled aggregation over daily datasets (`query_analytics`).
- **Find blind spots.** Which buyer queries never mention your brand (`get_prompt_mention_rates`)? Which prompts does a domain fail to get cited on (`get_content_opportunities`)?
- **Compare brands head-to-head** — 2–4 brands side by side on visibility, footprint, citations, and category ranking across AI engines (`compare_public_brands`).
- **Map category whitespace** — every topic in a category classified into strengths (covered / leading / close / defend) and opportunities (prioritize / gap / watch) for your brand (`get_category_whitespace`).
- **Track momentum.** Who is gaining or losing Share of Mention in AI answers, period over period (`get_category_brand_momentum`)?
- **See AI-search demand** — what people actually ask AI in your product space, which brands win those answers, and which demand territories each brand owns (`get_public_search_queries`).
- **Watch the AI shelf.** Which products AI recommends most across every category, who is climbing week over week (`list_public_shopping_boards`), and any single product's full AI profile (`get_public_shopping_product_detail`).
- **Score competition difficulty** — a 0–100 "keyword difficulty for the AI era" per topic (`get_topic_competition_difficulty`).
- **Profile AI perception.** How do AI models describe a brand? Canonical aspects, polarity, and verbatim evidence (`get_public_brand_perception`).
- **Audit AI readiness** — GEO site audits covering accessibility, structured data, content structure, and technical checks (`get_audit_detail`).

## Try asking

Once connected, ask your agent things like:

> - "How visible was my brand in AI answers over the last 30 days, and on which platform am I weakest?"
> - "Which buyer questions never mention us? Rank them by how often competitors show up instead."
> - "Compare Anker vs Soundcore visibility in the portable-audio category."
> - "Where's the whitespace in my category — which topics should we prioritize?"
> - "Which domains do AI engines cite most in my industry, and are we on any of them?"
> - "Which products are climbing the AI shopping shelf this week — and in which topics does reddit.com steer AI toward my competitor?"
> - "Run down my latest GEO site audit and list the critical issues."

## Quick start

**Prerequisite:** a GEOly account with a workspace and a monitored brand ([sign up](https://www.geoly.ai) and finish brand onboarding first — a brand-new workspace has no data to query yet).

Then: add the URL, make one tool call, sign in when the browser opens. That's the whole setup.

### Claude Code

```bash
claude mcp add --transport http geoly https://app.geoly.ai/api/mcp
```

### Cursor

[![Install MCP Server](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/install-mcp?name=geoly&config=eyJ1cmwiOiJodHRwczovL2FwcC5nZW9seS5haS9hcGkvbWNwIn0%3D)

Or add to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "geoly": {
      "url": "https://app.geoly.ai/api/mcp"
    }
  }
}
```

### Claude Desktop

Settings → Connectors → **Add custom connector**, then paste `https://app.geoly.ai/api/mcp` as the URL. Claude walks you through the OAuth consent in the browser.

### ChatGPT

In ChatGPT settings, enable developer mode for connectors, then add a custom connector with the URL `https://app.geoly.ai/api/mcp` and complete the OAuth sign-in. Yes — you can ask ChatGPT about your brand's visibility inside ChatGPT.

### VS Code (GitHub Copilot)

[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_GEOly_MCP-0098FF?logo=githubcopilot&logoColor=white)](https://vscode.dev/redirect/mcp/install?name=geoly&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapp.geoly.ai%2Fapi%2Fmcp%22%7D)

Or from the command line:

```bash
code --add-mcp '{"name":"geoly","type":"http","url":"https://app.geoly.ai/api/mcp"}'
```

### Codex CLI

Install through the GEOly plugin marketplace — the plugin registers the remote server and runs the OAuth flow on install, no manual config needed:

```bash
codex plugin marketplace add geoly-ai/codex-plugins
codex plugin add geoly-mcp@geoly
```

### Windsurf

Settings → MCP Configuration:

```json
{
  "mcpServers": {
    "geoly": {
      "serverUrl": "https://app.geoly.ai/api/mcp"
    }
  }
}
```

### Gemini CLI

```bash
gemini mcp add --transport http geoly https://app.geoly.ai/api/mcp
```

Or in `~/.gemini/settings.json`:

```json
{
  "mcpServers": {
    "geoly": {
      "httpUrl": "https://app.geoly.ai/api/mcp"
    }
  }
}
```

### Cline

Cline supports remote servers natively (note the camel-cased `streamableHttp`):

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

If the OAuth browser flow doesn't trigger in your Cline version, use the `mcp-remote` bridge below instead.

### Any other MCP client

Clients without native remote/OAuth support can bridge through [`mcp-remote`](https://www.npmjs.com/package/mcp-remote):

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

### GEOly CLI (terminals & CI)

The same tools, packaged as a command line built for agents — see [GEOly-Cli](https://github.com/geoly-ai/GEOly-Cli):

```bash
# macOS / Linux
curl -fsSL https://geoly.ai/install.sh | sh
# Windows
powershell -ExecutionPolicy Bypass -c "irm https://geoly.ai/install.ps1 | iex"

# No login step — the first call opens the browser to authorize
geoly call get_brand_overview --time_range 30d
```

## Authentication

| Track | How | Access |
| --- | --- | --- |
| **OAuth (default)** | Configure the URL with no credentials. The first call returns a standards-compliant challenge (RFC 9728 protected-resource metadata) that sends your client to a browser consent screen: sign in, choose which workspaces to share, and review the permission grid. | Per-resource read/write grants — read is preselected, write stays off unless you tick it |
| **Static token (CI / headless)** | Generate a `geom_...` token in your GEOly workspace settings and send it as `Authorization: Bearer geom_...`. | Always read-only |

Agencies and multi-workspace users: a single connection can span every workspace you belong to, or pin one with `https://app.geoly.ai/api/mcp?org_id=<id>` (get IDs from the `list_organizations` tool).

## Security & data access

- The server only reads data from workspaces you explicitly share at the OAuth consent screen — nothing beyond that scope.
- Write access is opt-in per resource on the consent screen and covers exactly 4 tools (create prompt / topic / competitor, trigger monitoring). Multi-workspace connections and static tokens are always read-only, no exceptions.
- Revoke a connection any time from your GEOly workspace settings; the client's cached credentials stop working immediately.
- The endpoint is stateless streamable HTTP over TLS. Nothing is installed or executed on your machine.

## Tools

60+ tools. The surface adapts to your access — single-brand connections skip the routing tools, read-only connections skip the write tools.

### Brand monitoring — overview & KPIs (4)

| Tool | What it returns |
| --- | --- |
| `get_brand_overview` | Headline KPIs: AIGVR score, mention/citation rates, per-platform stats — matches the in-app numbers |
| `get_brand_citations_daily` | Daily trend of AIGVR / mention rate / citation rate |
| `query_analytics` | Controlled aggregation (no SQL) over daily datasets — dimensions, metrics, filters, prompt-text subsets |
| `resolve_my_brand_public` | Bridge from your monitored brand to its public market-intelligence profile |

### Brand monitoring — prompts & answers (7)

| Tool | What it returns |
| --- | --- |
| `get_prompt_list` | Search/list monitored prompts with visibility stats |
| `get_prompt_detail` | One prompt in full: per-platform performance, AIGVR, Share of Model, competitor mentions |
| `get_prompt_record_summaries` | Latest monitoring record per platform for one prompt |
| `list_prompt_records` | Full execution history of one prompt over a time range, paginated — per-day trend work |
| `get_prompt_record_detail` | One monitored AI answer in full: text, citations, sentiment |
| `get_prompt_citations` | Citations for a prompt — raw or deduplicated URL list with share % |
| `get_prompt_mention_rates` | Per-prompt mention rate, ascending — blind-spot discovery |

### Brand monitoring — citations, domains & pages (5)

| Tool | What it returns |
| --- | --- |
| `get_citation_overview` | Citation domain distribution + ownership breakdown across the brand |
| `get_domain_detail` | One domain's citation profile: trend, pages, prompts, platforms, regions |
| `get_page_detail` | One page URL's citation detail: trend, prompt distribution, text snippets |
| `get_url_reference_detail` | A URL's references across citations and search sources |
| `get_content_opportunities` | Content-gap analysis: prompts where a domain has low or no citations |

### Brand monitoring — competitors, topics & sentiment (7)

| Tool | What it returns |
| --- | --- |
| `get_competitor_list` | Tracked competitors for the brand |
| `get_competitor_overview` | Cross-prompt competitor comparison |
| `get_competitor_cooccurrence` | Brand + competitor co-occurrence, with optional answer text |
| `get_platform_matrix` | Brand + competitors × platform matrix, or topics × platform |
| `get_topic_analytics` | Per-topic analysis: sentiment, competitors, response types, trends |
| `get_sentiment_dashboard` | Sentiment distribution, trends, platform comparison |
| `get_brand_mention_samples` | Recent AI answers mentioning the brand: raw text + sentiment + context |

### Site audits & GA4 (4)

| Tool | What it returns |
| --- | --- |
| `get_audit_list` | GEO site audits (AI-readiness diagnostics), paginated history |
| `get_audit_detail` | One audit in full: per-category scores, critical/warning/passed issues |
| `get_ga4_traffic_data` | GA4 integration: sessions, page views, distribution |
| `get_ga4_page_data` | GA4 page-level: views, sessions, bounce rate, traffic sources |

### Market intelligence — resolve & browse (4)

| Tool | What it returns |
| --- | --- |
| `search_public_entities` | Free-text resolver: brand / category / topic / product name or domain → public IDs (products via `include_products`) |
| `list_public_topics` | Browse public topics, with status/search filters |
| `list_public_locales` | Valid {country, language} pairs for an entity |
| `get_available_platforms` | Which AI platforms have data for a scope, ordered by volume |

### Market intelligence — topics (10)

| Tool | What it returns |
| --- | --- |
| `get_public_topic_overview` | Overview of one public topic |
| `get_public_topic_brand_leaderboard` | Brand leaderboard ranked by Share of Mention |
| `get_public_topic_som_trend` | Daily Share-of-Mention trend |
| `get_public_topic_prompt_matrix` | Prompt × brand heatmap (per-prompt SoM %) |
| `list_public_topic_prompts` | Every prompt under a topic, with leader brand & share |
| `get_public_topic_prompt_detail` | One prompt: per-brand breakdown, recent records, top citation domains |
| `get_public_topic_record_detail` | One public AI answer: snippeted text, citations, brands mentioned |
| `get_public_topic_citation_domains` | Citation-domain leaderboard for the topic |
| `get_public_topic_commerce` | Commerce aggregate: activation rate, price stats, retail channels |
| `get_topic_competition_difficulty` | AI-visibility difficulty 0–100, like SEO keyword difficulty |

### Market intelligence — brands (4)

| Tool | What it returns |
| --- | --- |
| `get_public_brand` | One public brand across topics, faceted: visibility, footprint, competitors, citations, ranking |
| `get_public_brand_perception` | AI perception profile: canonical aspects + polarity + evidence |
| `get_public_brand_perception_aspect_mentions` | Drill-down: source mentions behind one perception aspect |
| `compare_public_brands` | Side-by-side comparison of 2–4 brands on one facet |

### Market intelligence — categories, whitespace & momentum (3)

| Tool | What it returns |
| --- | --- |
| `get_public_category` | One product-space category, faceted: leaderboard, SoM trend, topics, citation domains |
| `get_category_whitespace` | Opportunity map: strengths (covered / leading / close / defend) vs opportunities (prioritize / gap / watch) |
| `get_category_brand_momentum` | Period-over-period Share-of-Mention change: risers vs fallers |

### Market intelligence — AI search queries (2)

| Tool | What it returns |
| --- | --- |
| `get_public_search_queries` | AI-search demand for a product space: queries, themes, brand landscape, demand territories |
| `get_public_search_query_detail` | Drill-down on one query or theme: brands, prompts, top sources |

### Market intelligence — shopping (4)

| Tool | What it returns |
| --- | --- |
| `list_public_shopping_boards` | The cross-category AI shelf leaderboard: hot / climbers / entrants with week-over-week rank moves |
| `get_public_shopping_product_detail` | One product's full AI analysis: shelves, weekly trend, rivals, channels |
| `list_public_shopping_products` | Shopping overview for a product-space slice: products, channels, price bands |
| `get_public_shopping_card_detail` | One product card preview: evidence, topics, prompts, retail offers |

### Public source domains (3)

| Tool | What it returns |
| --- | --- |
| `get_public_sources_overview` | Most-cited source domains across all public topics, each with its AI DA score |
| `get_public_source_domain_detail` | One citation source domain: coverage, co-occurring brands, optional full AI DA scorecard |
| `get_public_source_brand_conduit` | The topics where one source domain funnels AI attention toward one brand |

### Write tools (4)

Require write access granted on the OAuth consent screen. Static tokens and multi-workspace connections stay read-only.

| Tool | What it does |
| --- | --- |
| `create_prompt` | Create a new monitoring prompt |
| `create_topic` | Create a prompt topic |
| `create_competitor` | Add a competitor to track |
| `trigger_prompt` | Run monitoring for a prompt now (consumes credits) |

### Reports, discovery & routing (5)

| Tool | What it returns |
| --- | --- |
| `get_agent_ready_scans` | Agent Readiness scan history for the signed-in user |
| `get_agent_ready_scan_detail` | Full Agent Readiness scan result by ID |
| `list_organizations` | Workspaces the connection can access (multi-workspace mode) |
| `list_brands` | Brands in the workspace (multi-brand mode) |
| `get_current_date` | Server time, for date-range validation |

## Plans & access

| Tool group | Availability |
| --- | --- |
| Brand monitoring, audits, GA4, reports | Any active GEOly workspace |
| Market intelligence (topics, brands, categories, search queries, shopping) | Grow plan and above |
| Public source domains | All connections |
| Write tools | Write access granted at OAuth consent, single-workspace |

Heavy market-intelligence queries may count toward plan quotas, and `trigger_prompt` consumes monitoring credits. See [www.geoly.ai](https://www.geoly.ai) for plans.

## Troubleshooting

- **The first call returns 401** — that's the OAuth handshake by design; your client should open a browser. If it doesn't, the client lacks remote-OAuth support: bridge with `mcp-remote` (see above).
- **402 Payment Required** — the workspace subscription is inactive.
- **Market-intelligence tools are missing** — the topic / brand / category / search-query / shopping tool groups require the Grow plan or above. (The two public source domain tools are separate and available on all connections.)
- **Write tools are missing** — write access wasn't granted at consent, you're on a static token, or the connection spans multiple workspaces (writes are single-workspace only). Re-authenticate and tick the write permissions you need.
- **Opening the URL in a browser shows 405** — expected; the endpoint is POST-only streamable HTTP, not a web page.

## Related projects

| Project | What it is |
| --- | --- |
| [GEOly-Cli](https://github.com/geoly-ai/GEOly-Cli) | The same tools as a CLI, built for agents & CI |
| [agent-skills](https://github.com/geoly-ai/agent-skills) | Skills that teach AI agents to use this server correctly |
| [codex-plugins](https://github.com/geoly-ai/codex-plugins) | Codex plugin marketplace: this server + the geoly-mcp skill |

## Support

This repo documents the hosted GEOly MCP server. Issues with docs and config examples are welcome here; for account, plan, or data questions, reach us through [www.geoly.ai](https://www.geoly.ai).

## License

Documentation and examples in this repository are [MIT licensed](./LICENSE). The GEOly service itself is a commercial product.

---

**[www.geoly.ai](https://www.geoly.ai)** · [GEOly CLI](https://github.com/geoly-ai/GEOly-Cli) · © GEOly
