# SocialToAI: social media MCP server

[中文](README.zh.md)

**Social media MCP server: TikTok, Instagram, X (Twitter), Reddit, YouTube, LinkedIn, Facebook, Threads, Pinterest, Telegram, Xiaohongshu (RedNote), Douyin, Weibo, Bilibili, Zhihu, Kuaishou, WeChat Articles and WeChat Channels**, plus the Meta Ad Library and Google Ads Transparency Center. One hosted MCP address and one HTTPS API for public posts, comments, profiles and trending lists. No platform accounts, no cookies, no scraper to run.

SocialToAI is an API and MCP server that lets AI tools read publicly visible social media content (posts, comments, profiles and trends) from 18 platforms. It is billed in prepaid credits, and failed calls are free.

- MCP (Streamable HTTP): `https://mcp.socialtoai.com/mcp`
- REST API: `https://api.socialtoai.com/v1`
- 100 free credits on sign-up, no card. Credits never expire. You pay only for calls that return data.

## Connect in one line

This is a hosted remote server: there is nothing to install. ChatGPT, Claude, Claude Code, Codex and Cursor sign in with OAuth (an email code, then Allow), so no key is copied anywhere. Other clients send an API key in a header.

| Client | How |
| --- | --- |
| Claude Code | `claude mcp add --transport http socialtoai https://mcp.socialtoai.com/mcp`, then `/mcp` → socialtoai → Authenticate |
| Codex | `codex mcp add socialtoai --url https://mcp.socialtoai.com/mcp` (the browser opens to sign in) |
| Cursor | `~/.cursor/mcp.json`: `{"mcpServers":{"socialtoai":{"url":"https://mcp.socialtoai.com/mcp"}}}` |
| Claude (web, desktop) | Customize → Connectors → Add custom connector → `https://mcp.socialtoai.com/mcp` → Connect |
| ChatGPT | Settings → Apps (developer mode) → Create → `https://mcp.socialtoai.com/mcp`, authentication OAuth |
| Any other MCP client | URL `https://mcp.socialtoai.com/mcp`, header `Authorization: Bearer <YOUR_API_KEY>` |

Where nobody can sign in (a server, CI, a script), use an API key. Create one in the [console](https://socialtoai.com/console/keys/?utm_source=github&utm_medium=repository) (it is shown once), keep it in the environment variable `SOCIALTOAI_API_KEY`, and send it in the `Authorization: Bearer` header:

```bash
export SOCIALTOAI_API_KEY="<YOUR_API_KEY>"

# Claude Code, with a key instead of signing in
claude mcp add --transport http socialtoai https://mcp.socialtoai.com/mcp \
  --header "Authorization: Bearer $SOCIALTOAI_API_KEY"
```

```toml
# Codex: ~/.codex/config.toml
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

Already connected over OAuth and want to write a script? Ask your AI to call the `account` tool: its `for_scripts` field says where to create a key and how to send it. No answer ever contains a key.

Every client, step by step: [socialtoai.com/mcp](https://socialtoai.com/mcp/?utm_source=github&utm_medium=repository).

## Call it from code

Every action is one `POST` to `https://api.socialtoai.com/v1/<action>` with a JSON body:

```bash
curl https://api.socialtoai.com/v1/search \
  -H "Authorization: Bearer $SOCIALTOAI_API_KEY" \
  -H "Idempotency-Key: $(uuidgen)" \
  -H "Content-Type: application/json" \
  -d '{"platform": ["reddit", "xiaohongshu"], "query": "air fryer", "since": "2026-10-01"}'
```

`GET /v1/capabilities` and `POST /v1/find` need no key. The OpenAPI document is at [socialtoai.com/openapi.json](https://socialtoai.com/openapi.json), and the whole API for an AI to read at [socialtoai.com/llms.txt](https://socialtoai.com/llms.txt).

## Tools

| Tool | What it does | Billing |
| --- | --- | --- |
| `search` | Search a platform, or several at once; `since`/`until` dates on every platform | credits per call |
| `trending` | A platform's trending list | credits per call |
| `detail` | One post or video by link or id; transcripts where offered | credits per call |
| `comments` | The comments under a post, with likes and reply counts; commenters are anonymised | credits per call |
| `user_profile` | One account: name, bio, verification, followers | credits per call |
| `user_posts` | What one account published | credits per call |
| `find` | Find the right call by what it does, in English or Chinese; asked about docs, it answers the docs links | free |
| `call` | Platform-specific calls beyond the six actions (keyword indexes, creator marketplaces, ad libraries…) | credits per call |
| `capabilities` | Everything callable now, with parameters and prices | free |
| `account` | Balance, daily limits, how to buy, how to get an API key | free |
| `brand_buzz` | What people say about a brand on each platform, with sources | per run, quoted first |
| `comment_insights` | What the comments under your posts are about: topics, questions, purchase intent | per run, quoted first |
| `find_creators` | Creators in a niche on one platform, with size and fit | per run, quoted first |
| `account_watch` | What a set of accounts posted lately and which posts did better than usual | per run, quoted first |
| `creator_card` | One creator across platforms | per run, quoted first |
| `research` | Bulk link data, and viral posts for a topic | per run, quoted first |
| `watch` | Run a search or tool every day or week and deliver only what is new | each run as its calls |
| `watch_news` | What your watches found | free |

Every tool except `watch` is read-only: nothing changes on any platform or in your environment. Each answer states the credits charged and the balance in `billing.display`.

## Pricing

- Credits: $10 = 3,000 · $50 = 20,000 · $200 = 100,000, or any amount from $10 to $2,000. Card or Alipay, priced in USD. No subscription.
- A call costs 1 to 24 credits; most cost 2 (about $0.007). The price of every call is in `capabilities` and on [socialtoai.com/pricing](https://socialtoai.com/pricing/?utm_source=github&utm_medium=repository).
- Tools are priced by scale and quote their price before a costly run; `max_cost_credits` refuses, for free, anything that would cost more.
- Failed calls cost nothing. A search that ran and found nothing is charged.

## Links

- Site: [socialtoai.com](https://socialtoai.com/?utm_source=github&utm_medium=repository)
- Documentation: [socialtoai.com/docs](https://socialtoai.com/docs/?utm_source=github&utm_medium=repository)
- Connect your AI client: [socialtoai.com/mcp](https://socialtoai.com/mcp/?utm_source=github&utm_medium=repository)
- Status: [socialtoai.com/status](https://socialtoai.com/status/?utm_source=github&utm_medium=repository)
- Skill file for your AI: [github.com/socialtoai/socialtoai-skills](https://github.com/socialtoai/socialtoai-skills)
- Support: support@socialtoai.com

Published by the SocialToAI team. Data is content and account information publicly visible on each platform.
