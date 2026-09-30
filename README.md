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
- **Find blind spots.** Which buyer queries never mention your brand (`get_prompt_list` with `view="mention_rates"`)? Which domains does AI cite when it names a competitor but not you (`get_citation_overview` with `section="table"`, `gap_only=true`)?
- **Compare brands head-to-head** — 2–4 brands side by side on visibility, footprint, citations, and category ranking across AI engines (`get_public_brand` with `brand_ids`).
- **Map category whitespace** — every topic in a category classified into strengths (covered / leading / close / defend) and opportunities (prioritize / gap / watch) for your brand (`get_category_whitespace`).
- **Track momentum.** Who is gaining or losing Share of Mention in AI answers, period over period (`get_category_brand_momentum`)?
- **See AI-search demand** — what people actually ask AI in your product space, which brands win those answers, and which demand territories each brand owns (`get_public_search_queries`).
- **Watch the AI shelf.** Which products AI recommends most across every category, who is climbing week over week (`list_public_shopping_products` with `view="boards"`), and any single product's full AI profile (`get_public_shopping_product_detail`).
- **Score competition difficulty** — a 0–100 "keyword difficulty for the AI era" per topic (`get_public_topic` with `view="difficulty"`).
- **Profile AI perception.** How do AI models describe a brand? Canonical aspects, polarity, and verbatim evidence (`get_public_brand` with `view="perception"`).
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
- Write access is opt-in per resource on the consent screen and covers exactly 7 tools (create prompt / topic / competitor, archive or restore a prompt, edit prompt tags, move prompts into a topic, trigger monitoring). Multi-workspace connections and static tokens are always read-only, no exceptions.
- Revoke a connection any time from your GEOly workspace settings; the client's cached credentials stop working immediately.
- The endpoint is stateless streamable HTTP over TLS. Nothing is installed or executed on your machine.

## Tools

Up to 54 tools. The surface adapts to your access — single-brand connections skip the routing selectors, read-only connections skip the write tools, and the market-intelligence tools need the Grow plan (a read-only multi-workspace connection on Grow or above sees 47). Related reads share one tool: pick the view with its `view` / `mode` / `section` / `source` / `window_caliber` parameter. A parameter that belongs to a different view is rejected before the call runs.

### Brand monitoring — overview & KPIs (3)

| Tool | What it returns |
| --- | --- |
| `get_brand_overview` | Headline KPIs: AIGVR score, mention/citation rates, per-platform stats — matches the in-app numbers |
| `query_analytics` | Controlled aggregation (no SQL) over daily datasets — dimensions, metrics, filters, prompt-text subsets. Also the daily trend: `dataset="brand_citations_daily"` gives AIGVR / mention rate / citation rate per day per platform |
| `resolve_my_brand_public` | Bridge from your monitored brand to its public market-intelligence profile |

### Brand monitoring — prompts & answers (8)

| Tool | What it returns |
| --- | --- |
| `get_topic_list` | The brand's monitored topics with active / archived prompt counts — where topic ids come from |
| `get_prompt_list` | `view="table"` (default): search/list monitored prompts with visibility stats. `view="mention_rates"`: per-prompt mention rate, worst first — blind-spot discovery |
| `get_prompt_detail` | One prompt in full: per-platform performance, AIGVR, Share of Model, competitor mentions |
| `list_prompt_records` | Full execution history of one prompt over a time range, paginated — per-day trend work. `latest_per_platform=true`: just the latest record per platform |
| `list_brand_answers` | `view="table"` (default): every AI answer for the brand across all prompts, newest first, filterable by platform / topic / tag / country / brand. `view="mention_samples"`: recent answers mentioning the brand — raw text + sentiment + context |
| `get_prompt_record_detail` | One monitored AI answer in full: text, citations, sentiment |
| `get_prompt_citations` | Citations for a prompt — raw or deduplicated URL list with share % |
| `get_brand_search_queries` | Query fanout: the real web searches ChatGPT / Perplexity ran while answering your prompts, by `mode` (`overview`, `groups`, `query_detail`, `prompt_queries`) |

### Brand monitoring — citations, domains & pages (3)

| Tool | What it returns |
| --- | --- |
| `get_citation_overview` | Citation domain distribution + ownership breakdown across the brand. `section="board"` (default): totals, your share and rank, movers, trend, type share. `section="table"`: one page of cited root domains with share and change vs the previous window; `gap_only=true` keeps domains where a competitor is mentioned and you are not |
| `get_domain_detail` | One domain's citation profile: trend, pages, prompts, platforms, regions |
| `get_url_detail` | One page URL. `window_caliber="rolling"` (default): its references across citations and ChatGPT search sources over a rolling or custom window. `window_caliber="page"`: its citation detail on the in-app citations window — trend, prompt distribution, text snippets |

### Brand monitoring — competitors, topics & sentiment (7)

| Tool | What it returns |
| --- | --- |
| `get_competitor_list` | The brand library: tracked, suggested and removed brands, with 30-day mentions and spellings |
| `get_brand_board` | The in-app brand board: your brand vs confirmed competitors on visibility and share, optional daily trend |
| `get_platform_matrix` | Brand + auto-discovered competitors × platform, or topics × platform. `competitor_limit` (up to 20) + `include_totals=true` = the full cross-platform competitor comparison |
| `get_competitor_cooccurrence` | Brand + competitor co-occurrence, with optional answer text |
| `get_verdict` | AI Verdict, `view` required. `view="competitors"`: who AI prefers over you — per competitor, the answers where it was preferred, with change vs the previous window. `view="sources"`: cited domains in answers that carry verdict votes, with the share where your brand was judged negatively (default 7-day window) |
| `get_topic_analytics` | Per-topic analysis: sentiment, competitors, response types, trends |
| `get_sentiment_dashboard` | Sentiment distribution, trends, platform comparison |

### Site audits & traffic (3)

| Tool | What it returns |
| --- | --- |
| `get_audit_list` | GEO site audits (AI-readiness diagnostics), paginated history |
| `get_audit_detail` | One audit. `section="report"` (default): per-category scores, critical/warning/passed issues. `section="pages"`: per-page results |
| `get_traffic_data` | Site traffic, `source` required. `source="ga4"`: GA4 sessions, page views, AI-referred traffic (add `page_path` for one page's views, bounce rate, traffic sources). `source="cloudflare"`: AI-crawler requests, crawled paths, blocked crawlers |

### Market intelligence — resolve & browse (3)

| Tool | What it returns |
| --- | --- |
| `search_public_entities` | Free-text resolver: brand / category / topic / product name or domain → public IDs (products via `include_products`) |
| `list_public_topics` | Browse public topics, with status/search filters |
| `get_public_coverage` | Free discovery metadata, `view` required. `view="locales"`: valid {country, language} pairs for an entity. `view="platforms"`: which AI platforms have data for a scope, ordered by volume. `view="data_window"`: the published-batch time anchor and its 30 / 60 / 90-day windows |

### Market intelligence — topics (3)

| Tool | What it returns |
| --- | --- |
| `get_public_topic` | One public topic, by `view`: `overview` (default), `brand_leaderboard` (ranked by Share of Mention), `som_trend`, `prompt_matrix` (prompt × brand heatmap), `prompts` (every prompt, with leader brand & share), `citation_domains`, `commerce` (activation rate, price stats, retail channels), `difficulty` (AI-visibility difficulty 0–100, like SEO keyword difficulty — for a `topic_id`, a `prompt_id`, or a whole category via `product_space_id`) |
| `get_public_topic_prompt_detail` | One prompt: per-brand breakdown, recent records, top citation domains |
| `get_public_topic_record_detail` | One public AI answer: snippeted text, citations, brands mentioned |

### Market intelligence — brands (1)

| Tool | What it returns |
| --- | --- |
| `get_public_brand` | One public brand across topics, by `view`: facets `overview` (default), `visibility`, `footprint`, `competitors`, `citations`, `category_ranking`, `revenue`, `shopping`, `citation_totals`. Pass `brand_ids` (2–4) instead of `brand_id` for a side-by-side comparison on one facet (default `visibility`). `view="perception"`: AI perception profile — canonical aspects, polarity, evidence; `view="perception_mentions"` + `aspect`: the source mentions behind one aspect. `view="rank_citation"`: Google AI Overview rankings × AI citations — coverage, four search-counting quadrants, displacers; `view="rank_citation_rows"`: paginated per-search detail |

### Market intelligence — categories, whitespace & momentum (3)

| Tool | What it returns |
| --- | --- |
| `get_public_category` | One product-space category, faceted: leaderboard, SoM trend, topics, citation domains |
| `get_category_whitespace` | Opportunity map: strengths (covered / leading / close / defend) vs opportunities (prioritize / gap / watch) |
| `get_category_brand_momentum` | Period-over-period Share-of-Mention change: risers vs fallers |

### Market intelligence — AI search queries (1)

| Tool | What it returns |
| --- | --- |
| `get_public_search_queries` | AI-search demand for a product space, by `mode`: `territories` (which brand owns each demand territory), `product_spaces`, `query_detail` (one query: brands, prompts, top sources), `theme_detail` (one topic) |

### Market intelligence — shopping (2)

| Tool | What it returns |
| --- | --- |
| `list_public_shopping_products` | `view="products"` (default): one category's AI shelf — ranked products, channels, price bands (`product_space_id` required; `page` starts at 1). `view="boards"`: the cross-category AI shelf leaderboard — hot / climbers / entrants with week-over-week rank moves (`page` starts at 0) |
| `get_public_shopping_product_detail` | One product. `mode="full"` (default): its full AI analysis — shelves, weekly trend, rivals, channels. `mode="card"`: a cheap preview — evidence, topics, prompts, retail offers (`product_space_id` required) |

### Public source domains (3)

| Tool | What it returns |
| --- | --- |
| `get_public_sources_overview` | Most-cited source domains across all public topics, each with its AI DA score |
| `get_public_source_domain_detail` | One citation source domain: coverage, co-occurring brands, optional full AI DA scorecard |
| `get_public_source_brand_conduit` | The topics where one source domain funnels AI attention toward one brand |

### Write tools (7)

Require write access granted on the OAuth consent screen. Static tokens and multi-workspace connections stay read-only.

| Tool | What it does |
| --- | --- |
| `create_prompt` | Create a new monitoring prompt |
| `archive_prompt` | Archive a prompt (stops monitoring), or restore it with `restore=true` |
| `update_prompt_tags` | Add or remove tags on up to 500 prompts, or rename a tag brand-wide |
| `move_prompts_to_topic` | Move up to 500 prompts into a topic, or ungroup them |
| `create_topic` | Create a prompt topic |
| `create_competitor` | Add a competitor to track |
| `trigger_prompt` | Run monitoring for a prompt now (consumes credits) |

### Reports (1)

| Tool | What it returns |
| --- | --- |
| `get_agent_ready_scans` | Agent Readiness scans for the signed-in user: the scan history, or one full scan result with `scan_id` |

### Discovery & routing (6)

| Tool | What it returns |
| --- | --- |
| `get_brand_context` | One-shot orientation, free — call it first: brand, workspace & plan, today's business date, platforms, topics, tracked competitors, data window, remaining credits |
| `get_current_date` | Server time, for date-range validation |
| `get_quota` | MCP credits used / remaining this month and the reset date (always available) |
| `resolve_page_context` | Resolves a GEOly app URL the user is viewing into its entity scope and suggested tools |
| `list_organizations` | Workspaces the connection can access (multi-workspace mode) |
| `list_brands` | Brands in the workspace (multi-brand mode) |

## Plans & access

| Tool group | Availability |
| --- | --- |
| Brand monitoring, audits, site traffic (GA4 / Cloudflare), reports | Any active GEOly workspace |
| Market intelligence (topics, brands, categories, search queries, shopping) | Grow plan and above |
| Public source domains | All connections |
| Write tools | Write access granted at OAuth consent, single-workspace |

Heavy market-intelligence queries may count toward plan quotas, and `trigger_prompt` consumes monitoring credits. See [www.geoly.ai](https://www.geoly.ai) for plans.

## Troubleshooting

- **The first call returns 401** — that's the OAuth handshake by design; your client should open a browser. If it doesn't, the client lacks remote-OAuth support: bridge with `mcp-remote` (see above).
- **402 Payment Required** — the workspace subscription is inactive.
- **Market-intelligence tools are missing** — the topic / brand / category / search-query / shopping tool groups require the Grow plan or above. (The three public source domain tools are separate and available on all connections.)
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
