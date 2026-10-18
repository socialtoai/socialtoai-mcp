# SocialToAI MCP 服务

[English](README.md)

SocialToAI 是一个让 AI 工具读取社交媒体上公开可见内容（帖子、评论、账号、热榜）的 API 和 MCP 服务，覆盖 TikTok、Instagram、X、Reddit、YouTube、小红书、抖音、微博 等 18 个平台。按预付积分计费，失败不扣。

## 事实

| 事实 | |
| --- | --- |
| 平台（18 个） | TikTok、Instagram、X、Reddit、YouTube、Facebook、LinkedIn、Threads、Telegram、Pinterest、小红书、抖音、微博、公众号、B 站、知乎、快手、视频号 |
| 广告库（2 个） | Meta 广告库、Google 广告透明度中心 |
| 动作（6 个） | search、trending、detail、comments、user_profile、user_posts，另有平台独有接口目录（find / call） |
| 工具（7 个） | 品牌口碑、评论分析、账号监测、找达人、达人档案、批量链接数据、爆款发现 |
| API | https://api.socialtoai.com/v1 · MCP（Streamable HTTP）：https://mcp.socialtoai.com/mcp · 认证：API Key；ChatGPT 和 Claude 应用用 OAuth 登录 |
| 不带 Key 可用 | capabilities、find、OpenAPI |
| 价格 | $10 = 3,000 积分 · $50 = 20,000 · $200 = 100,000 · $10 到 $2,000 的任意整数金额 |
| 每次调用 | 多数调用 2 积分（范围 1–24）；低至每次 $0.002 |
| 免费 | 注册送 100 积分、不用绑卡；积分不过期、不订阅、不退款。 |
| 计费 | 成功的调用扣费，搜索正常但没搜到照扣；失败不扣。 |
| 限制 | 每把 Key 每秒 1 次，首次购买后每秒 5 次 |
| 支付 | 银行卡或支付宝，以美元计价 |
| 数据 | 平台上公开可见的内容和账号信息；评论者一律匿名 |
| 更新 | 2026-10-17 · 价格版本 pv_e5a1c68e5861 |

## 接入

这是一个托管的远程 MCP 服务，本机不用装任何东西。地址：`https://mcp.socialtoai.com/mcp`（Streamable HTTP）。ChatGPT 和 Claude 应用粘贴地址后登录授权即可，不用 Key；其余客户端用 API Key 认证。Key 在[控制台](https://socialtoai.com/zh/sign-in/?utm_source=github&utm_medium=repository)创建，只在创建时显示一次。下面的命令和配置里，Key 的位置都是环境变量 `SOCIALTOAI_KEY` 或占位符。

### Claude Code

位置：终端

1. 在控制台创建一把 Key，导出为环境变量 SOCIALTOAI_KEY。
2. 在终端里运行这条命令。
3. 打开 Claude Code 直接提问。随时输入 /mcp 查看连接状态。

```bash
export SOCIALTOAI_KEY="<YOUR_API_KEY>"

claude mcp add --transport http socialtoai \
  https://mcp.socialtoai.com/mcp \
  --header "Authorization: Bearer $SOCIALTOAI_KEY"

# check the connection
claude mcp list
```

### Claude app

位置：Customize → Connectors → Add custom connector

1. 在 Claude 网页版或桌面版打开 Customize → Connectors → Add custom connector。
2. 名称填 SocialToAI，粘贴地址后添加。高级设置留空。
3. 点 Connect，用邮箱验证码登录 SocialToAI，再点“允许”。
4. 在对话里打开这个连接器，直接提问。

- 名称（Name）：`SocialToAI`
- 远程 MCP 服务器地址（URL）：`https://mcp.socialtoai.com/mcp`

> 不用复制 Key：连接会出现在控制台的 API Keys 里，名字是 Claude，可以给它设每日上限或撤销。

> Claude 免费版只能添加一个自定义连接器。

### ChatGPT

位置：ChatGPT 设置 → Apps → Create

1. 在 ChatGPT 的设置里打开开发者模式，然后为自定义 MCP 服务器新建一个应用。
2. 名称填 SocialToAI，粘贴地址，认证方式选 OAuth，创建。
3. 用邮箱验证码登录 SocialToAI，再点“允许”。
4. 在对话里选中它，直接提问。

- 名称（Name）：`SocialToAI`
- MCP 服务器地址（URL）：`https://mcp.socialtoai.com/mcp`
- 认证方式（Authentication）：`OAuth`

> 需打开开发者模式。哪些套餐能开由 ChatGPT 决定，并且会调整，请以你账户的设置页为准。

> 不用复制 Key：连接会出现在控制台的 API Keys 里，名字是 ChatGPT，可以给它设每日上限或撤销。

### Cursor

位置：~/.cursor/mcp.json

1. 在控制台创建一把 Key，设为环境变量 SOCIALTOAI_KEY。
2. 把这段配置加到 ~/.cursor/mcp.json；只在某个项目里用，就放到项目的 .cursor/mcp.json。
3. 打开 Cursor 设置里的 MCP，确认 socialtoai 和它的工具已列出，然后在对话里提问。

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

位置：~/.codex/config.toml

1. 在控制台创建一把 Key，导出为环境变量 SOCIALTOAI_KEY。
2. 把这几行加到 ~/.codex/config.toml。
3. 打开 Codex 直接提问。Codex 从环境变量读取 Key，配置文件里不写 Key。

```toml
[mcp_servers.socialtoai]
url = "https://mcp.socialtoai.com/mcp"
bearer_token_env_var = "SOCIALTOAI_KEY"
```

### Trae

位置：MCP → 添加 → 手动配置

1. 在控制台创建一把 Key。
2. 在 Trae 里打开 MCP 面板，选择手动添加，粘贴这段配置。
3. 把 <YOUR_API_KEY> 换成你的 Key，确认后在对话里提问。

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

位置：设置 → MCP 服务器 → 添加

1. 在控制台创建一把 Key。
2. 在 Cherry Studio 打开“设置 → MCP 服务器”，按这些值添加一个服务器。
3. 把 <YOUR_API_KEY> 换成你的 Key，保存并启用，然后提问。

- 名称：`SocialToAI`
- 类型：`Streamable HTTP`
- URL：`https://mcp.socialtoai.com/mcp`
- 请求头：`Authorization=Bearer <YOUR_API_KEY>`

### Dify

位置：工具 → MCP → 添加 MCP 服务（HTTP）

1. 在控制台创建一把 Key。
2. 在 Dify 打开“工具 → MCP”，添加一个 HTTP 服务并填入这些值；请求头在高级设置里。
3. 把 <YOUR_API_KEY> 换成你的 Key 并保存，之后就能把这些工具加进 Agent 或工作流。

- 服务端点 URL：`https://mcp.socialtoai.com/mcp`
- 名称：`SocialToAI`
- 服务器标识符：`socialtoai`
- 请求头：`Authorization: Bearer <YOUR_API_KEY>`

### n8n

位置：MCP Client Tool 节点

1. 在控制台创建一把 Key。
2. 给 AI Agent 加一个 MCP Client Tool 节点，填入这些值。
3. 把 Key 存进 Bearer Auth 凭证里，不要写在节点上，然后运行工作流。

- Endpoint：`https://mcp.socialtoai.com/mcp`
- Server Transport：`HTTP Streamable`
- Authentication：`Bearer Auth`
- Bearer Token：`<YOUR_API_KEY>`

## 工具

| 工具 | 计费 |
| --- | --- |
| `search` | 按调用扣积分 |
| `trending` | 按调用扣积分 |
| `detail` | 按调用扣积分 |
| `comments` | 按调用扣积分 |
| `user_profile` | 按调用扣积分 |
| `user_posts` | 按调用扣积分 |
| `find` | 免费 |
| `call` | 按调用扣积分 |
| `capabilities` | 免费 |
| `account` | 免费 |
| `brand_buzz` | 按调用扣积分 |
| `comment_insights` | 按调用扣积分 |
| `find_creators` | 按调用扣积分 |
| `account_watch` | 按调用扣积分 |
| `creator_card` | 按调用扣积分 |
| `research` | 按调用扣积分 |

每个工具的参数和每次调用的积分见[文档](https://socialtoai.com/zh/docs/?utm_source=github&utm_medium=repository)。失败的调用不扣积分。

## 录制的示例

下面是在线上系统里真实运行过的问题，去掉了普通用户的名字。

### Reddit 和 X 上，大家对升降桌有哪些不满？

- Reddit 搜索 “standing desk”: 7 帖 · 2 积分
- X 搜索 “standing desk”: 11 帖 · 2 积分
- Reddit 评论分析 讨论最多的 3 帖的评论: 173 条评论 · 84 积分

两边都搜了，再读了 Reddit 上讨论最多的 3 帖下的 173 条评论。能判断的 162 条里有 55 条在抱怨，集中在这几点：

- **腰背和脖子疼** (12%): “Either position for 8 hours solid leaves my back hurting.” ([来源](https://www.reddit.com/r/BuyItForLife/comments/1ntedtv/do_standing_desks_actually_help_or_are_they/ngv5bws/))
- **忘了在坐和站之间切换** (11%): “I forget I have it most of the time...” ([来源](https://www.reddit.com/r/BuyItForLife/comments/1ntedtv/do_standing_desks_actually_help_or_are_they/ngt2t43/))
- **站久了脚疼** (8%): “My feet do get tired by the end of the day it’s better than the back pain.” ([来源](https://www.reddit.com/r/WFH/comments/1q1ia5q/i_bought_an_expensive_standing_desk_6_months_ago/nx6mz78/))

回放 · 录制于 2026-10-18，3 次请求，88 积分 ($0.29)

### 这周 TikTok 和 YouTube 上，家用咖啡什么内容在火？

- TikTok 爆款发现 “home coffee”，近 7 天: 59 条视频 · 36 积分
- YouTube 爆款发现 “home coffee”，近 7 天: 57 条视频 · 24 积分

近 7 天的 116 条视频里（能判断形式的 109 条），15 条起量（达到平时的 3 倍以上）。起量最多的几种形式，占比是占全部视频的比例：

- **教程** (24%): “October made me do it. 👻 Marshmallow Iced Latte Ingredients • 1 giant marshmallow” ([来源](https://www.tiktok.com/@probablymultitasking/video/7695837117548612878))
- **展示** (25%): “Setting up my home coffee bar (for now hehe) ☕️🧸🤎🕯️ what do we think?!” ([来源](https://www.tiktok.com/@dr.reemabassoumi/video/7695457439486971149))
- **故事** (20%): “I’ve been drinking mold for months.. PSA ALL AT HOME COFFEE MAKERS!” ([来源](https://www.tiktok.com/@godblessthismommess/video/7697482627208498446))

回放 · 录制于 2026-10-18，2 次请求，60 积分 ($0.2)

### 小红书上大家怎么聊露营咖啡？

- 小红书 品牌口碑 “露营咖啡”，近 90 天: 13 篇笔记 · 36 积分

找到的 13 条里有 8 条确实在说它：正面 100%，中性 0%，负面 0%。提到最多的是：

- **功能** (38%): “泰摩手冲套装，户外也能稳定萃取” ([来源](https://www.xiaohongshu.com/discovery/item/6ace5030000000000202b50a?xsec_token=YBG8sJk5ubaOiy4IR4aWFju_M8woOWi5f596-EyQu89_I%3D&xsec_source=app_share))
- **易用性** (25%): “意外比想象中方便。 撕开直接倒进水或牛奶里，冷水大约3秒就能化开” ([来源](https://www.xiaohongshu.com/discovery/item/6a7166a8000000002500db01?xsec_token=YBPlj8LOiespGe8OC5mJrELhKzAcNDnW_YlU9aDOcRrL0%3D&xsec_source=app_share))
- **质量** (13%): “亲手萃取的温柔质感 是成年人超治愈的低成本快乐” ([来源](https://www.xiaohongshu.com/discovery/item/6a5f63d40000000001033a6e?xsec_token=YB4BbvhOU2v5iwKzbmgNXN3JuvanMCovT7SnEUZ4w3TH0%3D&xsec_source=app_share))

回放 · 录制于 2026-10-18，1 次请求，36 积分 ($0.12)

## 链接

- 官网：https://socialtoai.com/zh/?utm_source=github&utm_medium=repository
- 接入你的 AI 客户端：https://socialtoai.com/zh/mcp/?utm_source=github&utm_medium=repository
- 价格：https://socialtoai.com/zh/pricing/?utm_source=github&utm_medium=repository
- 文档：https://socialtoai.com/zh/docs/?utm_source=github&utm_medium=repository
- 运行状态：https://socialtoai.com/zh/status/?utm_source=github&utm_medium=repository
- 更新日志：https://socialtoai.com/zh/changelog/?utm_source=github&utm_medium=repository
- 另一个仓库：https://github.com/socialtoai/socialtoai-skills
- 支持：support@socialtoai.com
