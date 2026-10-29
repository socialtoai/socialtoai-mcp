# SocialToAI：社交媒体公开数据 MCP 服务

[English](README.md)

**微博、抖音、小红书、B 站、知乎、快手、公众号、视频号公开数据 MCP**，海外的 TikTok、Instagram、X（Twitter）、Reddit、YouTube、LinkedIn、Facebook、Threads、Pinterest、Telegram 也在同一个地址里，另有 Meta 广告库和 Google 广告透明度中心。帖子、评论、账号、热榜，一个 MCP 地址、一套 HTTPS 接口接入，不用登录平台账号，不用 Cookie，不用自己跑爬虫。

SocialToAI 是一个让 AI 工具读取社交媒体上公开可见内容（帖子、评论、账号、热榜）的 API 和 MCP 服务，覆盖 18 个平台。按预付积分计费，失败不扣。

- MCP（Streamable HTTP）：`https://mcp.socialtoai.com/mcp`
- REST 接口：`https://api.socialtoai.com/v1`
- 注册送 100 积分，不用绑卡；积分永不过期；只为成功返回数据的调用付费。

## 一行接入

这是托管的远程服务，本机不用装任何东西。ChatGPT、Claude、Claude Code、Codex 和 Cursor 用 OAuth 登录授权（邮箱验证码，再点“允许”），哪里都不用复制 Key；其余客户端在请求头里带 API Key。

| 客户端 | 怎么接 |
| --- | --- |
| Claude Code | `claude mcp add --transport http socialtoai https://mcp.socialtoai.com/mcp`，然后 `/mcp` → socialtoai → Authenticate |
| Codex | `codex mcp add socialtoai --url https://mcp.socialtoai.com/mcp`（浏览器会打开登录页） |
| Cursor | `~/.cursor/mcp.json`：`{"mcpServers":{"socialtoai":{"url":"https://mcp.socialtoai.com/mcp"}}}` |
| Claude（网页版、桌面版） | Customize → Connectors → Add custom connector → `https://mcp.socialtoai.com/mcp` → Connect |
| ChatGPT | 设置 → Apps（开发者模式）→ Create → `https://mcp.socialtoai.com/mcp`，认证方式 OAuth |
| 其他 MCP 客户端 | 地址 `https://mcp.socialtoai.com/mcp`，请求头 `Authorization: Bearer <YOUR_API_KEY>` |

没人能登录的环境（服务器、CI、脚本）用 API Key：在[控制台](https://socialtoai.com/console/keys/?utm_source=github&utm_medium=repository)创建一把（只显示这一次），放进环境变量 `SOCIALTOAI_API_KEY`，放在 `Authorization: Bearer` 请求头里发：

```bash
export SOCIALTOAI_API_KEY="<YOUR_API_KEY>"

# Claude Code：用 Key 代替登录
claude mcp add --transport http socialtoai https://mcp.socialtoai.com/mcp \
  --header "Authorization: Bearer $SOCIALTOAI_API_KEY"
```

```toml
# Codex：~/.codex/config.toml
[mcp_servers.socialtoai]
url = "https://mcp.socialtoai.com/mcp"
bearer_token_env_var = "SOCIALTOAI_API_KEY"
```

```json
{
  "mcpServers": {
    "socialtoai": {
      "url": "https://mcp.socialtoai.com/mcp",
      "headers": { "Authorization": "Bearer ${env:SOCIALTOAI_API_KEY}" }
    }
  }
}
```

已经用 OAuth 连上了，又想写脚本？让 AI 调一次 `account` 工具：返回里的 `for_scripts` 写明在哪建 Key、怎么带上。任何回答里都不会出现 Key 本身。

每个客户端的分步说明：[socialtoai.com/zh/mcp](https://socialtoai.com/zh/mcp/?utm_source=github&utm_medium=repository)。

## 用代码调

每个动作都是向 `https://api.socialtoai.com/v1/<动作>` 发一个 `POST`，请求体是 JSON：

```bash
curl https://api.socialtoai.com/v1/search \
  -H "Authorization: Bearer $SOCIALTOAI_API_KEY" \
  -H "Idempotency-Key: $(uuidgen)" \
  -H "Content-Type: application/json" \
  -d '{"platform": ["xiaohongshu", "douyin", "weibo"], "query": "露营", "since": "2026-10-01"}'
```

`GET /v1/capabilities` 和 `POST /v1/find` 不用 Key。OpenAPI 文件在 [socialtoai.com/openapi.json](https://socialtoai.com/openapi.json)，给 AI 读的完整说明在 [socialtoai.com/llms.txt](https://socialtoai.com/llms.txt)。

## 工具

| 工具 | 做什么 | 计费 |
| --- | --- | --- |
| `search` | 搜一个或同时搜几个平台；每个平台都收 `since`/`until` 日期 | 按调用扣积分 |
| `trending` | 平台热榜 | 按调用扣积分 |
| `detail` | 按链接或编号取一条帖子或视频；支持的平台可带逐字稿 | 按调用扣积分 |
| `comments` | 帖子下的评论，带点赞数和回复数；评论者一律匿名 | 按调用扣积分 |
| `user_profile` | 一个账号：名称、简介、认证、粉丝数 | 按调用扣积分 |
| `user_posts` | 一个账号发过的内容 | 按调用扣积分 |
| `find` | 用中文或英文说要做什么，找到该用的调用；问文档时直接给文档地址 | 免费 |
| `call` | 六个动作之外的平台专属调用（关键词指数、达人平台、广告库等） | 按调用扣积分 |
| `capabilities` | 现在能调的全部内容，带参数和价格 | 免费 |
| `account` | 余额、每日上限、怎么充值、怎么拿 API Key | 免费 |
| `brand_buzz` | 各平台怎么评价一个品牌，带来源 | 按次，先报价 |
| `comment_insights` | 你的帖子下的评论在聊什么：话题、提问、购买意向 | 按次，先报价 |
| `find_creators` | 一个平台上某个领域的达人，带体量和匹配度 | 按次，先报价 |
| `account_watch` | 一组账号最近发了什么、哪几条明显好于平时 | 按次，先报价 |
| `creator_card` | 一位达人在各平台的账号 | 按次，先报价 |
| `research` | 批量链接数据，以及一个话题下的爆款 | 按次，先报价 |
| `watch` | 每天或每周跑一次搜索或工具，只推送新内容 | 每次按实际调用扣 |
| `watch_news` | 读监控找到的新内容 | 免费 |

除 `watch` 外，所有工具都是只读的：不会改动任何平台，也不会改动你的环境。每次返回都在 `billing.display` 里写明扣了多少、还剩多少。

## 价格

- 积分：$10（约 ¥72）= 3,000 · $50 = 20,000 · $200 = 100,000，也可以在 $10 到 $2,000 之间任意充值。银行卡或支付宝，按美元计价，无订阅。
- 每次调用 1 到 24 积分，大多数是 2 积分（约 $0.007）。每个调用的价格见 `capabilities` 和 [socialtoai.com/zh/pricing](https://socialtoai.com/zh/pricing/?utm_source=github&utm_medium=repository)。
- 工具按规模定价，花费较高的运行先报价；带上 `max_cost_credits`，超出就免费拒绝。
- 失败的调用不扣积分。搜索跑完但没有结果照常扣费。

## 链接

- 官网：[socialtoai.com](https://socialtoai.com/zh/?utm_source=github&utm_medium=repository)
- 文档：[socialtoai.com/zh/docs](https://socialtoai.com/zh/docs/?utm_source=github&utm_medium=repository)
- 接入你的 AI 客户端：[socialtoai.com/zh/mcp](https://socialtoai.com/zh/mcp/?utm_source=github&utm_medium=repository)
- 运行状态：[socialtoai.com/zh/status](https://socialtoai.com/zh/status/?utm_source=github&utm_medium=repository)
- 给 AI 用的技能文件：[github.com/socialtoai/socialtoai-skills](https://github.com/socialtoai/socialtoai-skills)
- 支持：support@socialtoai.com

由 SocialToAI 团队发布。数据为各平台上公开可见的内容和账号信息。
