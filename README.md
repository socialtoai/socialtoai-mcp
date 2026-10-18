# SocialToAI MCP server

[中文](README.zh.md)

SocialToAI is an API and MCP server that lets AI tools read publicly visible social media content (posts, comments, profiles and trends) from 18 platforms, including TikTok, Instagram, X (Twitter), Reddit, YouTube, Xiaohongshu (RedNote), Douyin, Weibo. It is billed in prepaid credits, and failed calls are free.

## Facts

| Fact | |
| --- | --- |
| Platforms (18) | TikTok, Instagram, X (Twitter), Reddit, YouTube, Facebook, LinkedIn, Threads, Telegram, Pinterest, Xiaohongshu (RedNote), Douyin, Weibo, WeChat Articles, Bilibili, Zhihu, Kuaishou, WeChat Channels |
| Ad libraries (2) | Meta Ad Library, Google Ads Transparency |
| Actions (6) | search, trending, detail, comments, user_profile, user_posts, plus a catalog of platform-specific interfaces (find / call) |
| Tools (7) | Brand buzz, Comment analysis, Account watch, Find creators, Creator profile, Bulk link data, Viral finder |
| API | https://api.socialtoai.com/v1 · MCP (Streamable HTTP): https://mcp.socialtoai.com/mcp · Auth: API key; OAuth sign-in for ChatGPT and Claude's apps |
| No key needed for | capabilities, find, OpenAPI |
| Price | $10 = 3,000 credits · $50 = 20,000 · $200 = 100,000 · any amount from $10 to $2,000 |
| Per call | most calls cost 2 credits (range 1–24); from $0.002 per call |
| Free | 100 credits on sign-up, no card. Credits never expire. No subscription. No refunds. |
| Charged | successful calls, including a search that ran and found nothing. Not charged: failed calls. |
| Limits | 1 request per second per key; 5 per second after the first purchase |
| Payment | card or Alipay, priced in USD |
| Data | content and account information publicly visible on the platform; commenters are anonymised |
| Updated | 2026-10-17 · price version pv_e5a1c68e5861 |

## Connect

This is a hosted remote MCP server: there is nothing to install. Address: `https://mcp.socialtoai.com/mcp` (Streamable HTTP). In ChatGPT and Claude's apps you paste the address and sign in, with no key; every other client authenticates with an API key. Create a key in the [console](https://socialtoai.com/sign-in/?utm_source=github&utm_medium=repository); it is shown once, when it is created. Wherever a key goes below there is the environment variable `SOCIALTOAI_KEY` or a placeholder.

### Claude Code

Where: Terminal

1. Create a key in the console and export it as SOCIALTOAI_KEY.
2. Run the command in your terminal.
3. Start Claude Code and ask. Type /mcp at any time to see the connection.

```bash
export SOCIALTOAI_KEY="<YOUR_API_KEY>"

claude mcp add --transport http socialtoai \
  https://mcp.socialtoai.com/mcp \
  --header "Authorization: Bearer $SOCIALTOAI_KEY"

# check the connection
claude mcp list
```

### Claude app

Where: Customize → Connectors → Add custom connector

1. In Claude on the web or the desktop app, open Customize → Connectors → Add custom connector.
2. Name it SocialToAI, paste the address and add it. Leave the advanced settings empty.
3. Press Connect. Sign in to SocialToAI with the code we email you and press Allow.
4. Switch the connector on in a chat and ask.

- Name: `SocialToAI`
- Remote MCP server URL: `https://mcp.socialtoai.com/mcp`

> No key to copy: the connection shows up under API keys in the console, named Claude, where you can give it a daily cap or revoke it.

> On the free plan Claude allows one custom connector.

### ChatGPT

Where: ChatGPT settings → Apps → Create

1. In ChatGPT, turn on developer mode in the settings, then create an app for a custom MCP server.
2. Name it SocialToAI, paste the address, choose OAuth and create it.
3. Sign in to SocialToAI with the code we email you and press Allow.
4. Pick it in a chat and ask.

- Name: `SocialToAI`
- MCP server URL: `https://mcp.socialtoai.com/mcp`
- Authentication: `OAuth`

> Developer mode has to be on. Which plans can turn it on is decided by ChatGPT and changes; check your account's settings.

> No key to copy: the connection shows up under API keys in the console, named ChatGPT, where you can give it a daily cap or revoke it.

### Cursor

Where: ~/.cursor/mcp.json

1. Create a key in the console and set it as the environment variable SOCIALTOAI_KEY.
2. Add the server to ~/.cursor/mcp.json, or to .cursor/mcp.json in one project.
3. Open Cursor's settings, find MCP, and check that socialtoai is listed with its tools. Then ask in the chat.

```json
{
  "mcpServers": {
    "socialtoai": {
      "url": "https://mcp.socialtoai.com/mcp",
      "headers": {
        "Authorization": "Bearer ${env:SOCIALTOAI_KEY}"
      }
    }
  }
}
```

### Codex

Where: ~/.codex/config.toml

1. Create a key in the console and export it as SOCIALTOAI_KEY.
2. Add these lines to ~/.codex/config.toml.
3. Start Codex and ask. Codex reads the key from the environment variable; the file never holds it.

```toml
[mcp_servers.socialtoai]
url = "https://mcp.socialtoai.com/mcp"
bearer_token_env_var = "SOCIALTOAI_KEY"
```

### Trae

Where: MCP → Add → Configure manually

1. Create a key in the console.
2. In Trae, open the MCP panel, choose to add a server manually and paste the configuration.
3. Replace <YOUR_API_KEY> with your key, confirm, and ask in the chat.

```json
{
  "mcpServers": {
    "socialtoai": {
      "url": "https://mcp.socialtoai.com/mcp",
      "headers": {
        "Authorization": "Bearer <YOUR_API_KEY>"
      }
    }
  }
}
```

### Cherry Studio

Where: Settings → MCP Servers → Add

1. Create a key in the console.
2. In Cherry Studio, open Settings → MCP Servers and add a server with these values.
3. Replace <YOUR_API_KEY> with your key, save, switch the server on and ask.

- Name: `SocialToAI`
- Type: `Streamable HTTP`
- URL: `https://mcp.socialtoai.com/mcp`
- Headers: `Authorization=Bearer <YOUR_API_KEY>`

### Dify

Where: Tools → MCP → Add MCP Server (HTTP)

1. Create a key in the console.
2. In Dify, open Tools → MCP, add an HTTP server and fill in these values; the header is under the advanced settings.
3. Replace <YOUR_API_KEY> with your key and save. The tools can then be added to an agent or a workflow.

- Server URL: `https://mcp.socialtoai.com/mcp`
- Name: `SocialToAI`
- Server identifier: `socialtoai`
- Headers: `Authorization: Bearer <YOUR_API_KEY>`

### n8n

Where: MCP Client Tool node

1. Create a key in the console.
2. Add an MCP Client Tool node to an AI Agent and fill in these values.
3. Store your key in the Bearer Auth credential, not in the node, then run the workflow.

- Endpoint: `https://mcp.socialtoai.com/mcp`
- Server transport: `HTTP Streamable`
- Authentication: `Bearer Auth`
- Bearer token: `<YOUR_API_KEY>`

## Tools

| Tool | Billing |
| --- | --- |
| `search` | credits per call |
| `trending` | credits per call |
| `detail` | credits per call |
| `comments` | credits per call |
| `user_profile` | credits per call |
| `user_posts` | credits per call |
| `find` | free |
| `call` | credits per call |
| `capabilities` | free |
| `account` | free |
| `brand_buzz` | credits per call |
| `comment_insights` | credits per call |
| `find_creators` | credits per call |
| `account_watch` | credits per call |
| `creator_card` | credits per call |
| `research` | credits per call |

The parameters of each tool and the credits of each call are in the [documentation](https://socialtoai.com/docs/?utm_source=github&utm_medium=repository). A failed call is not charged.

## Recorded examples

Questions that were run on the live system, with the names of ordinary users removed.

### What do people complain about with standing desks on Reddit and X?

- Searched Reddit “standing desk”: 7 posts · 2 credits
- Searched X “standing desk”: 11 posts · 2 credits
- Ran Comment analysis on Reddit comments under the 3 most-discussed threads: 173 comments · 84 credits

I searched both, then read 173 comments under the 3 most-discussed Reddit threads. 55 of the 162 that could be judged complain about something; the complaints cluster here:

- **Back and neck pain** (12%): “Either position for 8 hours solid leaves my back hurting.” ([source](https://www.reddit.com/r/BuyItForLife/comments/1ntedtv/do_standing_desks_actually_help_or_are_they/ngv5bws/))
- **Forgetting to switch between sitting and standing** (11%): “I forget I have it most of the time...” ([source](https://www.reddit.com/r/BuyItForLife/comments/1ntedtv/do_standing_desks_actually_help_or_are_they/ngt2t43/))
- **Sore feet from standing** (8%): “My feet do get tired by the end of the day it’s better than the back pain.” ([source](https://www.reddit.com/r/WFH/comments/1q1ia5q/i_bought_an_expensive_standing_desk_6_months_ago/nx6mz78/))

Replay · recorded 2026-10-18, 3 requests, 88 credits ($0.29)

### What home coffee content is taking off on TikTok and YouTube this week?

- Ran Viral finder on TikTok “home coffee”, last 7 days: 59 videos · 36 credits
- Ran Viral finder on YouTube “home coffee”, last 7 days: 57 videos · 24 credits

From 116 videos posted in the last 7 days (109 with a format the analysis could name), 15 took off (3 times the usual or more). The formats that took off most, each with its share of all the videos:

- **How-to** (24%): “October made me do it. 👻 Marshmallow Iced Latte Ingredients • 1 giant marshmallow” ([source](https://www.tiktok.com/@probablymultitasking/video/7695837117548612878))
- **Showcase** (25%): “Setting up my home coffee bar (for now hehe) ☕️🧸🤎🕯️ what do we think?!” ([source](https://www.tiktok.com/@dr.reemabassoumi/video/7695457439486971149))
- **Story** (20%): “I’ve been drinking mold for months.. PSA ALL AT HOME COFFEE MAKERS!” ([source](https://www.tiktok.com/@godblessthismommess/video/7697482627208498446))

Replay · recorded 2026-10-18, 2 requests, 60 credits ($0.2)

### What are people on Xiaohongshu saying about camping coffee?

- Ran Brand buzz on Xiaohongshu “露营咖啡”, last 90 days: 13 notes · 36 credits

8 of the 13 posts found are about it: 100% positive, 0% neutral, 0% negative. What they mention most:

- **Features** (38%): “泰摩手冲套装，户外也能稳定萃取” ([source](https://www.xiaohongshu.com/discovery/item/6ace5030000000000202b50a?xsec_token=YBG8sJk5ubaOiy4IR4aWFju_M8woOWi5f596-EyQu89_I%3D&xsec_source=app_share))
- **Ease of use** (25%): “意外比想象中方便。 撕开直接倒进水或牛奶里，冷水大约3秒就能化开” ([source](https://www.xiaohongshu.com/discovery/item/6a7166a8000000002500db01?xsec_token=YBPlj8LOiespGe8OC5mJrELhKzAcNDnW_YlU9aDOcRrL0%3D&xsec_source=app_share))
- **Quality** (13%): “亲手萃取的温柔质感 是成年人超治愈的低成本快乐” ([source](https://www.xiaohongshu.com/discovery/item/6a5f63d40000000001033a6e?xsec_token=YB4BbvhOU2v5iwKzbmgNXN3JuvanMCovT7SnEUZ4w3TH0%3D&xsec_source=app_share))

Replay · recorded 2026-10-18, 1 request, 36 credits ($0.12)

## Links

- Site: https://socialtoai.com/?utm_source=github&utm_medium=repository
- Connect your AI client: https://socialtoai.com/mcp/?utm_source=github&utm_medium=repository
- Pricing: https://socialtoai.com/pricing/?utm_source=github&utm_medium=repository
- Documentation: https://socialtoai.com/docs/?utm_source=github&utm_medium=repository
- Status: https://socialtoai.com/status/?utm_source=github&utm_medium=repository
- Changelog: https://socialtoai.com/changelog/?utm_source=github&utm_medium=repository
- The other repository: https://github.com/socialtoai/socialtoai-skills
- Support: support@socialtoai.com
