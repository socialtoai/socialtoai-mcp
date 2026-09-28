# SocialToAI MCP

<img src="logo.png" width="96" alt="SocialToAI logo">

**Social media data for your AI.** A hosted remote MCP server (Streamable HTTP) that lets Claude, Cursor, Cherry Studio, Trae, Cline and other MCP clients search and read public posts, comments, trending boards and creator profiles from **Xiaohongshu (RedNote), Douyin, Kuaishou, Weibo, Zhihu, Bilibili, WeChat Official Accounts, X, Reddit and YouTube**, with source links.

> This repository contains documentation and client configuration only. The service is hosted at `https://mcp.socialtoai.com/mcp`; its source code is not public.

[中文说明](#中文说明) · [Website](https://socialtoai.com/) · [Quickstart](https://socialtoai.com/docs/quickstart/) · [Pricing](https://socialtoai.com/pricing/) · [Status](https://socialtoai.com/status/)

## Get a key

Sign in at <https://socialtoai.com/console/> with email or Google (no card). New accounts get free starter credits, enough to try a Xiaohongshu search, a note and its comments. Set a daily limit per key.

## Connect

Endpoint: `https://mcp.socialtoai.com/mcp` · Transport: Streamable HTTP · Auth: `Authorization: Bearer <key>`, `X-API-Key: <key>` or `?key=<key>` (no OAuth).

**Claude Code**

```bash
claude mcp add --transport http socialtoai https://mcp.socialtoai.com/mcp --header "Authorization: Bearer $SOCIALTOAI_API_KEY"
```

**Cursor** (`.cursor/mcp.json`; one-click “Add to Cursor” button: <https://socialtoai.com/docs/quickstart/#cursor>)

```json
{ "mcpServers": { "socialtoai": { "url": "https://mcp.socialtoai.com/mcp", "headers": { "Authorization": "Bearer ${env:SOCIALTOAI_API_KEY}" } } } }
```

**Cherry Studio** (Settings → MCP Servers → Add → Import from JSON)

```json
{ "mcpServers": { "socialtoai": { "type": "streamableHttp", "url": "https://mcp.socialtoai.com/mcp", "headers": { "Authorization": "Bearer YOUR_SOCIALTOAI_KEY" } } } }
```

**Cline**

```json
{ "mcpServers": { "socialtoai": { "type": "streamableHttp", "url": "https://mcp.socialtoai.com/mcp", "headers": { "Authorization": "Bearer YOUR_SOCIALTOAI_KEY" } } } }
```

**Any client that only takes a URL:** `https://mcp.socialtoai.com/mcp?key=YOUR_SOCIALTOAI_KEY` (treat this URL as a secret).

Optional packs (enable them on your key first): add `packs=kol,index,raw` to the URL.

## Tools

| Tool | What it does |
|---|---|
| `search` | Search public posts or accounts on one or up to five platforms |
| `trending` | Read a platform's current trending board |
| `detail` | Read one public post, video, note or article in full |
| `comments` | Read public comments and replies on one item, with pagination |
| `user_profile` | Read one public creator or account profile |
| `user_posts` | Read public posts from one creator or account |
| `account` · `ping` · `capabilities` | Balance, connectivity and per-platform capabilities (free) |

Packs: **KOL** (creator search, audience, quotes and performance; Xiaohongshu keyword index), **Index** (Douyin content trends, creator comparison), **Raw** (whitelisted native read operations with an explicit `max_cost`). All tools are read-only. Current availability per operation: <https://socialtoai.com/status/>.

## Pricing

Pay per call, 10 credits = ¥1. Fixed public prices per platform and verb (0.1–1.5 credits for core calls). Successful and confirmed-empty calls are charged; failed calls are free. No subscription, credits never expire, top up from ¥50. Full list: <https://socialtoai.com/pricing/>.

## Boundaries

Reads supported public content only. It does not publish, read private content or bypass access controls. Your AI client does the analysis; responses include source links so claims can be checked.

## Support

support@socialtoai.com · [Privacy](https://socialtoai.com/privacy/) · [Terms](https://socialtoai.com/terms/)

---

## 中文说明

SocialToAI 是面向 AI 的社交数据网关。一个远程 MCP 地址，让你的 AI 客户端搜索和读取小红书、抖音、快手、微博、知乎、B 站、微信公众号、X、Reddit、YouTube 的公开帖子、评论、热榜和博主数据，回答带出处。

1. 在 <https://socialtoai.com/console/> 用邮箱或 Google 登录，注册即送免费额度（够试一次小红书搜索、详情和评论），无需绑卡。
2. 复制 MCP 地址 `https://mcp.socialtoai.com/mcp?key=你的Key`，或使用请求头 `Authorization: Bearer 你的Key`，粘进 Cherry Studio、Trae、Cursor、Claude 等客户端（配置见上文）。
3. 像平时一样提问：“帮我看看小红书上最近关于露营咖啡的笔记，大家在吐槽什么？”

按次付费，10 credits = ¥1，失败不扣费，额度不过期，¥50 起充。完整价格：<https://socialtoai.com/zh/pricing/> · 文档：<https://socialtoai.com/zh/docs/quickstart/>
