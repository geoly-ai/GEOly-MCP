[English](./README.md) | 简体中文

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/geoly-ai/GEOly-MCP/main/assets/geoly-icon-dark.png">
  <img src="https://raw.githubusercontent.com/geoly-ai/GEOly-MCP/main/assets/geoly-icon.png" align="right" width="72" alt="GEOly logo">
</picture>

# GEOly MCP Server

**[GEOly](https://www.geoly.ai)** 官方远程 MCP server —— 把 AI 品牌可见度（GEO）数据接进你的 agent。GEOly 持续追踪品牌在各大 AI 引擎（ChatGPT、Perplexity、Google AI Mode、Google AI Overview、Gemini、Copilot）回答中的提及与引用情况；这个 server 把这些数据——可见度 KPI、竞品份额、引用信源、行业市场情报、站点审计——直接送进 Claude、Cursor、Codex、VS Code 或任何 MCP 客户端。

云端托管、streamable HTTP、浏览器内 OAuth。一个 URL，本地零部署：

```
https://app.geoly.ai/api/mcp
```

## 你的 agent 能做什么

- **拉取与应用内完全一致的 KPI** —— 分 AI 平台的 AIGVR 得分、提及率、引用率（`get_brand_overview`），每日趋势，以及免 SQL 的受控聚合分析（`query_analytics`）。
- **发现盲区。** 哪些买家问题从不提及你的品牌（`get_prompt_mention_rates`）？哪些 prompt 你的域名拿不到引用（`get_content_opportunities`）？
- **品牌硬碰硬对比** —— 2–4 个品牌在可见度、覆盖面、引用、品类排名上并排比较（`compare_public_brands`）。
- **绘制品类空白地图** —— 把品类下每个话题划分为优势区（covered / leading / close / defend）与机会区（prioritize / gap / watch）（`get_category_whitespace`）。
- **追踪动量。** 谁在 AI 回答中的 Share of Mention 环比上升、谁在下滑（`get_category_brand_momentum`）？
- **看清 AI 搜索需求** —— 用户在你的产品领域实际问 AI 什么、哪些品牌赢下了这些回答、每个需求词根领地被谁占住（`get_public_search_queries`）。
- **盯住 AI 货架。** 全品类 AI 最爱推荐哪些商品、谁在周环比蹿升（`list_public_shopping_boards`），任一商品的完整 AI 面孔（`get_public_shopping_product_detail`）。
- **量化竞争难度** —— 每个话题一个 0–100 的"AI 时代关键词难度"（`get_topic_competition_difficulty`）。
- **剖析 AI 认知画像。** AI 模型如何描述一个品牌？认知维度、正负极性、原文证据（`get_public_brand_perception`）。
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
claude mcp add --transport http geoly https://app.geoly.ai/api/mcp
```

### Cursor

[![Install MCP Server](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/install-mcp?name=geoly&config=eyJ1cmwiOiJodHRwczovL2FwcC5nZW9seS5haS9hcGkvbWNwIn0%3D)

或写入 `~/.cursor/mcp.json`：

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

设置 → Connectors → **Add custom connector**，URL 填 `https://app.geoly.ai/api/mcp`，随后按浏览器里的 OAuth 授权流程走完即可。

### ChatGPT

在 ChatGPT 设置里开启 connectors 的开发者模式，添加自定义 connector，URL 填 `https://app.geoly.ai/api/mcp`，完成 OAuth 登录。没错——你可以在 ChatGPT 里问自己品牌在 ChatGPT 里的可见度。

### VS Code（GitHub Copilot）

[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_GEOly_MCP-0098FF?logo=githubcopilot&logoColor=white)](https://vscode.dev/redirect/mcp/install?name=geoly&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapp.geoly.ai%2Fapi%2Fmcp%22%7D)

或命令行一键添加：

```bash
code --add-mcp '{"name":"geoly","type":"http","url":"https://app.geoly.ai/api/mcp"}'
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
      "serverUrl": "https://app.geoly.ai/api/mcp"
    }
  }
}
```

### Gemini CLI

```bash
gemini mcp add --transport http geoly https://app.geoly.ai/api/mcp
```

或写入 `~/.gemini/settings.json`：

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

Cline 已原生支持远程 server（注意 `streamableHttp` 是驼峰写法）：

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

如果你的 Cline 版本没能自动弹出 OAuth 浏览器授权，改用下面的 `mcp-remote` 桥接。

### 其他任意 MCP 客户端

不支持远程 OAuth 的客户端可以用 [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) 桥接：

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

代理商与多工作区用户：一条连接可以覆盖你所属的全部工作区，也可以用 `https://app.geoly.ai/api/mcp?org_id=<id>` 锁定单个工作区（id 可通过 `list_organizations` 工具获取）。

## 安全与数据边界

- server 只读取你在 OAuth 授权页明确共享的工作区数据，绝不越界。
- 写权限在授权页按资源逐项开启，且只覆盖 4 个工具（建 prompt / 话题 / 竞品、触发监控）。多工作区连接与静态 token 恒为只读，无例外。
- 随时可在 GEOly 工作区设置中吊销连接，客户端缓存的凭据立即失效。
- 端点是 TLS 上的无状态 streamable HTTP，不在你的机器上安装或执行任何东西。

## 工具

60+ 个工具。工具集合会随访问权限自适应——单品牌连接不出现路由类工具，只读连接不出现写入类工具。完整清单见 [英文版 README](https://github.com/geoly-ai/GEOly-MCP/blob/main/README.md#tools)，这里列分组概览：

| 分组 | 数量 | 内容（代表工具） |
| --- | --- | --- |
| 品牌监控 — 总览与 KPI | 4 | AIGVR/提及率/引用率（`get_brand_overview`）、受控聚合（`query_analytics`） |
| 品牌监控 — prompt 与回答 | 7 | prompt 详情（`get_prompt_detail`）、执行历史（`list_prompt_records`）、盲区发现（`get_prompt_mention_rates`） |
| 品牌监控 — 引用、域名与页面 | 5 | 引用域名分布（`get_citation_overview`）、内容机会（`get_content_opportunities`） |
| 品牌监控 — 竞品、话题与情感 | 7 | 竞品对比（`get_competitor_overview`）、平台矩阵（`get_platform_matrix`）、情感面板（`get_sentiment_dashboard`） |
| 站点审计与 GA4 | 4 | GEO 审计详情（`get_audit_detail`）、GA4 流量（`get_ga4_traffic_data`） |
| 市场情报 — 检索与浏览 | 4 | 实体解析（`search_public_entities`）、话题浏览（`list_public_topics`） |
| 市场情报 — 话题 | 10 | 品牌榜（`get_public_topic_brand_leaderboard`）、竞争难度（`get_topic_competition_difficulty`） |
| 市场情报 — 品牌 | 4 | 多品牌对比（`compare_public_brands`）、AI 认知画像（`get_public_brand_perception`） |
| 市场情报 — 品类 | 3 | 空白机会地图（`get_category_whitespace`）、品牌动量（`get_category_brand_momentum`） |
| 市场情报 — AI 搜索 query | 2 | AI 搜索需求全景+需求领地（`get_public_search_queries`） |
| 市场情报 — 购物 | 4 | AI 货架榜（`list_public_shopping_boards`）、商品全景（`get_public_shopping_product_detail`） |
| 公开信源域名 | 3 | 最常被引信源榜（`get_public_sources_overview`）、源×品牌导管（`get_public_source_brand_conduit`） |
| 写入工具 | 4 | 建 prompt/话题/竞品、立即触发监控（`trigger_prompt`） |
| 报告、发现与路由 | 5 | Agent Readiness 报告（`get_agent_ready_scan_detail`）、工作区列表（`list_organizations`） |

## 套餐与访问

| 工具组 | 可用范围 |
| --- | --- |
| 品牌监控、审计、GA4、报告 | 任何有效 GEOly 工作区 |
| 市场情报（话题/品牌/品类/搜索 query/购物） | Grow 及以上套餐 |
| 公开信源域名 | 所有连接 |
| 写入工具 | OAuth 授权页勾选写权限、单工作区连接 |

重查询类市场情报工具可能计入套餐额度；`trigger_prompt` 消耗监控 credits。套餐详情见 [www.geoly.ai](https://www.geoly.ai)。

## 常见问题排查

- **首次调用返回 401** —— 这是 OAuth 握手的设计行为，客户端应自动弹浏览器；如果没弹，说明客户端不支持远程 OAuth，用上文的 `mcp-remote` 桥接。
- **402 Payment Required** —— 工作区订阅未生效。
- **看不到市场情报工具** —— 话题/品牌/品类/搜索 query/购物这几组工具需要 Grow 及以上套餐。（公开信源域名那 2 个工具不受此限制，所有连接可用。）
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
