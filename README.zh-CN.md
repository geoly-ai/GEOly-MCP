[English](./README.md) | 简体中文

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/geoly-ai/GEOly-MCP/main/assets/geoly-icon-dark.png">
  <img src="https://raw.githubusercontent.com/geoly-ai/GEOly-MCP/main/assets/geoly-icon.png" align="right" width="72" alt="GEOly logo">
</picture>

# GEOly MCP Server

**[GEOly](https://www.geoly.ai)** 官方远程 MCP server —— 把 AI 品牌可见度（GEO）数据接进你的 agent。GEOly 持续追踪品牌在各大 AI 引擎（ChatGPT、Perplexity、Google AI Mode、Google AI Overview、Gemini、Copilot）回答中的提及与引用情况；这个 server 把这些数据——可见度 KPI、竞品份额、引用信源、行业市场情报、站点审计——直接送进 Claude、Cursor、Codex、VS Code 或任何 MCP 客户端。

云端托管、streamable HTTP、浏览器内 OAuth。一个 URL，本地零部署：

```
https://app.geoly.ai/api/mcp/v1
```

`/api/mcp/v1` 是 GEOly MCP v1 的带版本正式地址；不带版本的 `https://app.geoly.ai/api/mcp` 完全等同、继续保留，已有配置无需改动。

## 你的 agent 能做什么

- **拉取与应用内完全一致的 KPI** —— 分 AI 平台的 AIGVR 得分、提及率、引用率（`get_brand_overview`），每日趋势，以及免 SQL 的受控聚合分析（`query_analytics`）。
- **发现盲区。** 哪些买家问题从不提及你的品牌（`get_prompt_list`，`view="mention_rates"`）？AI 提到竞品却没提你时，引用的是哪些域名（`get_citation_overview`，`section="table"`、`gap_only=true`）？
- **品牌硬碰硬对比** —— 2–4 个品牌在可见度、覆盖面、引用、品类排名上并排比较（`get_public_brand`，传 `brand_ids`）。
- **绘制品类空白地图** —— 把品类下每个话题划分为优势区（covered / leading / close / defend）与机会区（prioritize / gap / watch）（`get_category_whitespace`）。
- **追踪动量。** 谁在 AI 回答中的 Share of Mention 环比上升、谁在下滑（`get_category_brand_momentum`）？
- **看清 AI 搜索需求** —— 用户在你的产品领域实际问 AI 什么、哪些品牌赢下了这些回答、每个需求词根领地被谁占住（`get_public_search_queries`）。
- **盯住 AI 货架。** 全品类 AI 最爱推荐哪些商品、谁在周环比蹿升（`list_public_shopping_products`，`view="boards"`），任一商品的完整 AI 面孔（`get_public_shopping_product_detail`）。
- **量化竞争难度** —— 每个话题一个 0–100 的"AI 时代关键词难度"（`get_public_topic`，`view="difficulty"`）。
- **剖析 AI 认知画像。** AI 模型如何描述一个品牌？认知维度、正负极性、原文证据（`get_public_brand`，`view="perception"`）。
- **审计 AI 就绪度** —— 覆盖可访问性、结构化数据、内容结构、技术项的 GEO 站点审计（`get_audit_detail`）。

## 接好之后可以这样问

> - "过去 30 天我的品牌在 AI 回答里可见度如何？哪个平台最弱？"
> - "哪些买家问题从来不提我们？按竞品出现频率排个序。"
> - "对比一下 Anker 和 Soundcore 在便携音频品类的 AI 可见度。"
> - "我的品类里空白机会在哪？哪些话题应该优先攻？"
> - "我所在行业 AI 引擎最爱引用哪些网站？我们上榜了吗？"
> - "这周 AI 购物货架上谁在蹿升？reddit.com 又在哪些话题里把 AI 导向我的竞品？"
> - "把我最近一次 GEO 站点审计过一遍，列出严重问题。"

## 快速开始

**前置条件：** 一个 GEOly 账号 + 工作区 + 已在监控中的品牌（先去 [www.geoly.ai](https://www.geoly.ai) 注册并完成品牌 onboarding——全新工作区还没有数据可查）。

之后：加 URL → 发起一次工具调用 → 浏览器弹出后登录，整个接入就这三步。

### Claude Code

```bash
claude mcp add --transport http geoly https://app.geoly.ai/api/mcp/v1
```

### Cursor

[![Install MCP Server](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/install-mcp?name=geoly&config=eyJ1cmwiOiJodHRwczovL2FwcC5nZW9seS5haS9hcGkvbWNwIn0%3D)

或写入 `~/.cursor/mcp.json`：

```json
{
  "mcpServers": {
    "geoly": {
      "url": "https://app.geoly.ai/api/mcp/v1"
    }
  }
}
```

### Claude Desktop

设置 → Connectors → **Add custom connector**，URL 填 `https://app.geoly.ai/api/mcp/v1`，随后按浏览器里的 OAuth 授权流程走完即可。

### ChatGPT

在 ChatGPT 设置里开启 connectors 的开发者模式，添加自定义 connector，URL 填 `https://app.geoly.ai/api/mcp/v1`，完成 OAuth 登录。没错——你可以在 ChatGPT 里问自己品牌在 ChatGPT 里的可见度。

### VS Code（GitHub Copilot）

[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_GEOly_MCP-0098FF?logo=githubcopilot&logoColor=white)](https://vscode.dev/redirect/mcp/install?name=geoly&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapp.geoly.ai%2Fapi%2Fmcp%22%7D)

或命令行一键添加：

```bash
code --add-mcp '{"name":"geoly","type":"http","url":"https://app.geoly.ai/api/mcp/v1"}'
```

### Codex CLI

通过 GEOly 插件市场安装——插件自动注册远程 server 并在安装时完成 OAuth，无需手动配置：

```bash
codex plugin marketplace add geoly-ai/codex-plugins
codex plugin add geoly-mcp@geoly
```

### Windsurf

设置 → MCP Configuration：

```json
{
  "mcpServers": {
    "geoly": {
      "serverUrl": "https://app.geoly.ai/api/mcp/v1"
    }
  }
}
```

### Gemini CLI

```bash
gemini mcp add --transport http geoly https://app.geoly.ai/api/mcp/v1
```

或写入 `~/.gemini/settings.json`：

```json
{
  "mcpServers": {
    "geoly": {
      "httpUrl": "https://app.geoly.ai/api/mcp/v1"
    }
  }
}
```

### Cline

Cline 已原生支持远程 server（注意 `streamableHttp` 是驼峰写法）：

```json
{
  "mcpServers": {
    "geoly": {
      "type": "streamableHttp",
      "url": "https://app.geoly.ai/api/mcp/v1"
    }
  }
}
```

如果你的 Cline 版本没能自动弹出 OAuth 浏览器授权，改用下面的 `mcp-remote` 桥接。

### 其他任意 MCP 客户端

不支持远程 OAuth 的客户端可以用 [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) 桥接：

```json
{
  "mcpServers": {
    "geoly": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://app.geoly.ai/api/mcp/v1"]
    }
  }
}
```

### GEOly CLI（终端与 CI）

同一套工具，做成为 agent 设计的命令行形态 —— 见 [GEOly-Cli](https://github.com/geoly-ai/GEOly-Cli)：

```bash
# macOS / Linux
curl -fsSL https://geoly.ai/install.sh | sh
# Windows
powershell -ExecutionPolicy Bypass -c "irm https://geoly.ai/install.ps1 | iex"

# 无需 login 步骤——首次调用自动打开浏览器授权
geoly call get_brand_overview --time_range 30d
```

## 认证

| 方式 | 用法 | 权限 |
| --- | --- | --- |
| **OAuth（默认）** | 只配 URL、不配任何凭据。首次调用返回标准挑战（RFC 9728 protected-resource metadata），客户端自动跳浏览器授权页：登录、选择要共享的工作区、核对权限矩阵。 | 按资源逐项授予读/写——读默认勾选，写默认关闭、需要手动勾选 |
| **静态 token（CI / 无头环境）** | 在 GEOly 工作区设置中生成 `geom_...` token，以 `Authorization: Bearer geom_...` 携带。 | 恒为只读 |

代理商与多工作区用户：一条连接可以覆盖你所属的全部工作区，也可以用 `https://app.geoly.ai/api/mcp/v1?org_id=<id>` 锁定单个工作区（id 可通过 `list_organizations` 工具获取）。

## 安全与数据边界

- server 只读取你在 OAuth 授权页明确共享的工作区数据，绝不越界。
- 写权限在授权页按资源逐项开启，且只覆盖 7 个工具（建 prompt / 话题 / 竞品、归档或恢复 prompt、编辑 prompt 标签、把 prompt 移入话题、触发监控）。多工作区连接与静态 token 恒为只读，无例外。
- 随时可在 GEOly 工作区设置中吊销连接，客户端缓存的凭据立即失效。
- 端点是 TLS 上的无状态 streamable HTTP，不在你的机器上安装或执行任何东西。

## 工具

最多 53 个工具。工具集合会随访问权限自适应——单品牌连接不出现路由选择类工具，只读连接不出现写入类工具，市场情报工具需要 Grow 套餐（Grow 及以上的只读多工作区连接可见 47 个）。相关读取合并在同一个工具里，用 `view` / `mode` / `section` / `source` / `window_caliber` 参数选视图；传了属于其他视图的参数，调用执行前就会被拒绝。完整清单见 [英文版 README](https://github.com/geoly-ai/GEOly-MCP/blob/main/README.md#tools)，这里列分组概览：

| 分组 | 数量 | 内容（代表工具） |
| --- | --- | --- |
| 品牌监控 — 总览与 KPI | 3 | AIGVR/提及率/引用率（`get_brand_overview`）、受控聚合与每日趋势（`query_analytics`，`dataset="brand_citations_daily"`） |
| 品牌监控 — prompt 与回答 | 8 | prompt 列表与盲区发现（`get_prompt_list`，`view="table"` / `"mention_rates"`）、执行历史（`list_prompt_records`，`latest_per_platform=true` 取各平台最新一条）、品牌回答表与提及样本（`list_brand_answers`，`view="table"` / `"mention_samples"`） |
| 品牌监控 — 引用、域名与页面 | 3 | 引用域名看板与域名表（`get_citation_overview`，`section="board"` / `"table"`，`gap_only=true` 只看缺口）、单 URL 详情（`get_url_detail`，`window_caliber="rolling"` / `"page"`） |
| 品牌监控 — 竞品、话题与情感 | 7 | 品牌榜（`get_brand_board`）、平台矩阵与竞品对比（`get_platform_matrix`，`competitor_limit` + `include_totals=true`）、AI 裁决（`get_verdict`，`view="competitors"` 竞品偏好榜 / `"sources"` 被引信源）、情感面板（`get_sentiment_dashboard`） |
| 站点审计与站点流量 | 3 | GEO 审计报告与逐页结果（`get_audit_detail`，`section="report"` / `"pages"`）、站点流量（`get_traffic_data`，`source="ga4"` / `"cloudflare"`） |
| 市场情报 — 检索与浏览 | 3 | 实体解析（`search_public_entities`）、话题浏览（`list_public_topics`）、语言区/平台/数据窗口（`get_public_coverage`，`view="locales"` / `"platforms"` / `"data_window"`） |
| 市场情报 — 话题 | 3 | 话题多视图（`get_public_topic`：品牌榜 `view="brand_leaderboard"`、竞争难度 `view="difficulty"` 等 8 个视图）、prompt 与单条回答下钻（`get_public_topic_prompt_detail`、`get_public_topic_record_detail`） |
| 市场情报 — 品牌 | 1 | 公开品牌多视图（`get_public_brand`）：多品牌对比（传 `brand_ids`）、AI 认知画像（`view="perception"`）、排名 × AI 引用（`view="rank_citation"`） |
| 市场情报 — 品类 | 3 | 空白机会地图（`get_category_whitespace`）、品牌动量（`get_category_brand_momentum`） |
| 市场情报 — AI 搜索 query | 1 | AI 搜索需求全景+需求领地（`get_public_search_queries`，`mode="territories"` / `"query_detail"`） |
| 市场情报 — 购物 | 2 | 品类货架与 AI 货架榜（`list_public_shopping_products`，`view="products"` / `"boards"`）、商品全景（`get_public_shopping_product_detail`，`mode="full"` / `"card"`） |
| 公开信源域名 | 3 | 最常被引信源榜（`get_public_sources_overview`）、源×品牌导管（`get_public_source_brand_conduit`） |
| 写入工具 | 7 | 建 prompt/话题/竞品、归档 prompt（`archive_prompt`）、批量改标签、移动到话题、立即触发监控（`trigger_prompt`） |
| 报告 | 1 | Agent Readiness 扫描历史与详情（`get_agent_ready_scans`，带 `scan_id` 取详情） |
| 发现与路由 | 6 | 一次性定位（`get_brand_context`）、额度查询（`get_quota`）、工作区列表（`list_organizations`） |

## 版本

- **`https://app.geoly.ai/api/mcp/v1` 就是 GEOly MCP v1**——即上面列出的工具面。不带版本的 `https://app.geoly.ai/api/mcp` 是同一处理器、行为完全一致，保留给现有配置。每个响应都带 `GEOly-MCP-Version: 1` 头，initialize 时服务端也会声明版本（`serverInfo.version` 为 `1.0.0`，server instructions 首段写明版本与政策）。
- **版本政策与 GEOly Agent API（`GEOly-API-Version: 1`）同一条**：旧版本留着跑、不维护、不下线。真有破坏性改动才会在新路径 `/api/mcp/v2` 上开 v2，`/api/mcp/v1`（以及 `/api/mcp`）原样保留 v1；新增工具、视图、可选参数都在 v1 上做，不升版本。目前没有 v2。
- **0.7.0 精简之前的旧工具名**（34 个，例如 `compare_public_brands`、`list_citation_domains`）不再出现在工具列表里，在 v1 上**可调用到 2026-11-30，之后移除**（调用返回 `Tool … not found`）。用旧名调用成功时，返回顶层带可机读的 `_deprecated` 块：`sunset` 是移除日期，`use` 是应改用的写法。旧名 → 新写法见 [geoly.ai/open/mcp](https://geoly.ai/zh/open/mcp#migration) 与技能的 [`tools-catalog`](https://github.com/geoly-ai/agent-skills/blob/main/skills/geoly-mcp/references/tools-catalog.md)。
- **已移除**：`get_competitor_overview`、`get_brand_citations_daily`、`get_content_opportunities` 无法一对一替换，到 2026-11-30 前只返回免费的 `TOOL_REMOVED` 错误并给出替代写法（`get_platform_matrix` `dimension="competitor"`、`query_analytics` `dataset="brand_citations_daily"`、`get_citation_overview` `section="table"` + `gap_only=true`）。
- **弃用 mode 已删除**：`get_brand_search_queries` 的 `roots` / `root_detail` / `topic_roots` 与 `get_public_search_queries` 的 `queries` / `themes` / `brand_landscape` / `prompt_map` 不再存在，两个工具的 `mode` 现在必填。

## 套餐与访问

| 工具组 | 可用范围 |
| --- | --- |
| 品牌监控、审计、站点流量（GA4 / Cloudflare）、报告 | 任何有效 GEOly 工作区 |
| 市场情报（话题/品牌/品类/搜索 query/购物） | Grow 及以上套餐 |
| 公开信源域名 | 所有连接 |
| 写入工具 | OAuth 授权页勾选写权限、单工作区连接 |

重查询类市场情报工具可能计入套餐额度；`trigger_prompt` 消耗监控 credits。套餐详情见 [www.geoly.ai](https://www.geoly.ai)。

## 常见问题排查

- **首次调用返回 401** —— 这是 OAuth 握手的设计行为，客户端应自动弹浏览器；如果没弹，说明客户端不支持远程 OAuth，用上文的 `mcp-remote` 桥接。
- **402 Payment Required** —— 工作区订阅未生效。
- **看不到市场情报工具** —— 话题/品牌/品类/搜索 query/购物这几组工具需要 Grow 及以上套餐。（公开信源域名那 3 个工具不受此限制，所有连接可用。）
- **看不到写入工具** —— 授权时没勾写权限、在用静态 token、或连接跨了多个工作区（写入仅限单工作区）。重新授权并勾选所需的写权限。
- **浏览器直接打开 URL 显示 405** —— 正常现象；端点是 POST-only 的 streamable HTTP，不是网页。

## 相关项目

| 项目 | 说明 |
| --- | --- |
| [GEOly-Cli](https://github.com/geoly-ai/GEOly-Cli) | 同一套工具的 CLI 形态，为 agent 与 CI 而生 |
| [agent-skills](https://github.com/geoly-ai/agent-skills) | 教 AI agent 正确使用本 server 的技能包 |
| [codex-plugins](https://github.com/geoly-ai/codex-plugins) | Codex 插件市场：本 server + geoly-mcp skill |

## 支持

本仓库是托管版 GEOly MCP server 的文档主页。文档与配置示例问题欢迎提 issue；账号、套餐、数据类问题请通过 [www.geoly.ai](https://www.geoly.ai) 联系我们。

## 许可

本仓库中的文档与示例代码以 [MIT 协议](./LICENSE) 开源。GEOly 服务本身为商业产品。

---

**[www.geoly.ai](https://www.geoly.ai)** · [GEOly CLI](https://github.com/geoly-ai/GEOly-Cli) · © GEOly
