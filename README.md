> 如需Clash版本 → [请点击此处](https://github.com/LingJingMaster/Clash-Rules)

---

# 小火箭 Shadowrocket Config

一份开箱即用的 小火箭 Shadowrocket 规则配置
- 支持前沿的苹果智能（Apple Intelligence）
- 在新规下的香港券商访问
- 导入后添加自己的节点或订阅即可使用

## 快速开始

1. 复制配置文件的 Raw 链接：
   `https://raw.githubusercontent.com/youngtreebig-droid/Shadowrocket-Rules/refs/heads/main/Shadowrocket.conf`
2. 打开 Shadowrocket → 配置 → 右上角 `+` → 粘贴链接 → 下载
3. 点击已下载的配置，设为使用中（✔️）
4. 首页添加你自己的节点或订阅
5. 连通性测试，选择可用节点连接

或者扫描二维码

<img width="200" height="200" alt="ctool-2026-02-26-17-13-16" src="https://github.com/user-attachments/assets/22f1b4f7-3265-493c-9e5a-2b662924ed2f" />

## 分流规则

| 优先级 | 服务 | 默认策略 |
|--------|------|----------|
| 1 | 🧱 WebRTC 防泄露（UDP 3478 / 5349 / 19302-19309 的 STUN 流量） | 国内 IP 直连，其余 REJECT |
| 2 | 🏠 局域网 / 私有网络（blackmatrix7 `Lan`、Loyalsoldier `private`） | DIRECT（固定） |
| 3 | 微信、1Password 等精确直连；国内 AI（通义、智谱、Kimi、豆包、DeepSeek 等，VPSDance `cn.list`） | DIRECT（固定） |
| 4 | 🤖 AI 服务（大陆需代理的海外 AI：ChatGPT / Codex、Claude / Claude Code、Gemini / AI Studio / NotebookLM、Grok / Grok Bot、Kiro、Copilot、Cursor、Windsurf、Perplexity、Mistral、Meta AI、Hugging Face、OpenRouter、Midjourney、Suno、ElevenLabs、苹果智能等百余家服务） | 美国节点 |
| 5 | 🛑 广告 / 追踪拦截（Loyalsoldier `reject` 约 19 万条 + blackmatrix7 `BlockHttpDNS`；Netflix 风控域名例外放行） | REJECT |
| 6 | 📧 邮件服务（IMAP / POP3 / SMTP） | DIRECT，可切换节点 |
| 7 | 📹 油管视频（含 YouTube 翻译 API；放在谷歌之前，否则 googlevideo.com 视频流会被谷歌分组截走） | 节点选择 |
| 8 | 🔍 谷歌服务（Gemini 等 Google AI 已前置到 🤖 AI 服务） | 美国节点，可手动切日本、香港节点 |
| 9 | 🎵 TikTok（抖音国际版 / CapCut 等） | 美国节点 |
| 10 | 🎬 Netflix（blackmatrix7 `Shadowrocket/Netflix/Netflix.list`） | 香港节点 |
| 11 | 🔞 日本AV（DMM / FANZA、S1、Moodyz 等片商） | 日本节点 |
| 12 | missav（missav 及 JAV 聚合 / 在线观看站） | 香港节点 |
| 13 | 哔哩哔哩 | DIRECT（固定） |
| 14 | 📲 电报消息、🐱 代码托管（GitHub、GitLab、Atlassian） | PROXY（固定） |
| 15 | Ⓜ️ 微软服务 | 美国节点 |
| 16 | 🏦 汇丰香港 / 香港银行 / 📈 券商服务 | DIRECT（固定） |
| 17 | 🍎 苹果推送、🍏 苹果服务（另加 Loyalsoldier `apple`、`icloud`） | DIRECT |
| 18 | 🇨🇳 国内域名（blackmatrix7 `ChinaMax_Domain`） | DIRECT（固定） |
| 19 | 🌐 境外域名（blackmatrix7 `Global_Domain`、`GlobalMedia_Domain`，Loyalsoldier `proxy`） | PROXY（固定） |
| 20 | 含 IP 的规则：国内 IP / 关键字（blackmatrix7 `ChinaMax`）→ DIRECT；境外 IP / 关键字（blackmatrix7 `Global`、`GlobalMedia`，Loyalsoldier `telegramcidr`）→ PROXY | 见左 |
| 21 | GEOIP CN | DIRECT |
| 22 | 🐟 漏网之鱼（所有列表都没匹配到的流量） | 香港节点 |

规则从上往下匹配，命中即停。排序原则：
- 精确、小范围的规则放前面，大而全的国内 / 境外列表放后面，避免大列表抢走需要单独分流的服务（例如 Gemini 被谷歌分组截走）
- 只匹配域名的规则放前面，含 IP 的规则（`IP-CIDR`、`GEOIP`）放最后。匹配 IP 前必须先做 DNS 解析，放在前面会让每个请求都先等解析，变慢
- 局域网规则的 IP 部分带 `no-resolve`，不会触发解析，所以可以放最前
- 你自己的分组（AI、油管、谷歌、TikTok、Netflix、日本AV、missav、微软、苹果等）全部排在 Global、Loyalsoldier `proxy` 等境外代理列表之前，命中分组就按分组选的节点走，不会被改成 PROXY
- 谷歌账号登录（`accounts.youtube.com`、`apis.google.com`）和谷歌自家统计 / 崩溃上报（Firebase、Crashlytics、`gvt2.com` 等）单独放在最前面，固定走 🔍 谷歌服务：保证登录时所有谷歌域名出口一致，避免触发"验证设备"，也避免 Gmail 因统计请求被拦而报"无网络连接"；谷歌广告仍然拦截
- 广告拦截放在 🤖 AI 服务之后、谷歌和国内直连等大列表之前：AI 依赖的统计域名（statsig、datadog 等）不会被误杀，doubleclick、admob 等广告域名又能先被拦截


## 默认策略

| 服务 | 默认策略 | 可选策略 |
|------|----------|----------|
| 📧 邮件服务 | DIRECT | 节点选择、PROXY、美国 / 日本 / 香港节点 |
| 🎬 Netflix | 🇭🇰 香港节点 | 节点选择、PROXY、DIRECT、日本 / 美国节点 |
| 🔍 谷歌服务 | 🇺🇸 美国节点 | 🇭🇰 香港节点、🇯🇵 日本节点、节点选择、PROXY、DIRECT |
| 🤖 AI 服务 | 🇺🇸 美国节点 | 节点选择、PROXY、DIRECT、日本节点、香港节点 |
| 🎵 TikTok | 🇺🇸 美国节点 | 节点选择、PROXY、DIRECT、日本节点、香港节点 |
| 🔞 日本AV | 🇯🇵 日本节点 | PROXY、DIRECT、REJECT、日本 / 美国 / 香港节点 |
| missav | 🇭🇰 香港节点 | PROXY、DIRECT、REJECT、日本 / 美国 / 香港节点 |
| 📹 油管视频 | 🚀 节点选择 | PROXY、DIRECT、美国 / 日本 / 香港节点 |
| Ⓜ️ 微软服务 | 🇺🇸 美国节点 | 节点选择、PROXY、DIRECT、日本 / 香港节点 |
| 🍎 苹果推送 | DIRECT | 节点选择、PROXY、美国 / 日本 / 香港节点 |
| 🍏 苹果服务 | DIRECT | 节点选择、PROXY、美国 / 日本 / 香港节点 |
| 🐟 漏网之鱼 | 🇭🇰 香港节点 | PROXY、DIRECT、美国 / 日本 / 香港节点 |

- 🚀 节点选择 默认 PROXY（即首页当前选中的节点）
- 🇯🇵 日本节点 / 🇺🇸 美国节点 为手动选择组，默认分别为 `TOKYO2-VLESS`、`美国-166.0.26.50 (53467测试)`，不做倍率过滤，可在组内改选
- 🌐 其他节点 / 🇭🇰 / 🇹🇼 自动组默认过滤掉倍率 ≥ 1 的节点（节点名需带 `[倍率]`，如 `[6.0]`），需要时可在「节点选择」里手动选择
- 固定策略（不在策略组中，直接写入规则）：
- 📲 电报消息、🐱 代码托管、🌐 其他境外服务（Twitter / X、Facebook、Instagram、Reddit、Wikipedia、Discord、Spotify、Disney+、Twitch 等，不在分组里的都算） → PROXY，即首页当前选中的节点
- 🏦 汇丰香港、🏦 香港银行、📈 券商服务、🔒 国内服务、🏠 私有网络 → DIRECT
- 🔐 1Password → DIRECT（上游 EliDoves/Proxy `1Password.list` + 官方文档补充域名；1Password 无公开固定 IP，按域名匹配）
- 🧱 DNS 防泄露、WebRTC 防泄露、🛑 广告 / 追踪（Loyalsoldier `reject`） → REJECT







## 规则集来源

- [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) — 主要规则集；国内直连用 `ChinaMax`（含 China、ChinaIPs 等全部国内规则）
- [Loyalsoldier/surge-rules](https://github.com/Loyalsoldier/surge-rules) — `reject`（广告 / 追踪拦截）、`private`（局域网域名）、`apple`、`icloud`，每日构建。国内直连只用 blackmatrix7 ChinaMax：其说明要求引用 ChinaMax 后不再叠加其他国内规则，所以没有引用 Loyalsoldier `direct`、`cncidr`
- [VPSDance/ai-proxy-rules](https://github.com/VPSDance/ai-proxy-rules) `cn.list` — 国内 AI 直连
- 境外代理：blackmatrix7 `Global`、`GlobalMedia`（流媒体），Loyalsoldier `proxy`（包含 `gfw` 全部域名）、`telegramcidr`，统一走 PROXY；放在 ChinaMax 之后，国内可直连的学术期刊、阿里云国际站等优先直连
- [iab0x00/ProxyRules](https://github.com/iab0x00/ProxyRules) — AI 服务补充规则
- 🤖 AI 服务（规则整体放在 `Google.list`、`Microsoft.list` 之前，避免 Gemini、Copilot 被 🔍 谷歌服务 / Ⓜ️ 微软服务提前匹配；国内 AI 不在此组）：
  - [VPSDance/ai-proxy-rules](https://github.com/VPSDance/ai-proxy-rules) `rules/shadowrocket/global.list`：海外 AI 汇总，百余家厂商，每日同步
  - [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community) `geosite:category-ai-!cn`，Shadowrocket 格式由 [ddranic/ios-proxy-rules](https://github.com/ddranic/ios-proxy-rules) 每日转换
  - [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) `OpenAI`、`Copilot`、`Gemini`、`Claude`
  - 上游 LingJingMaster `AI.list`（苹果智能等）
  - `conf` 内手动补充上游未收录的域名：Kiro 的 `kiro.aws.dev`、Cognito 登录端点（来源 [Kiro 防火墙文档](https://kiro.dev/docs/privacy-and-security/firewalls/)）；Grok 的 `grokusercontent.com`、`spacex.ai`（来源 [xAI 登录域名文档](https://docs.x.ai/x-subscription/sign-in-domains)）；以及 Le Chat、Krea、Higgsfield、Luma、Black Forest Labs、Tensor.Art、Warp、TypingMind、ChatPDF、Elicit、Consensus
  - 注意：上游为保证 AI 服务完整可用，也收录了少量通用域名（如 `storage.googleapis.com`、`apis.google.com`、`intercom.io`，以及含 `sentry`、`datadog` 关键字的域名），这些流量同样会走 🤖 AI 服务
- `Mail.list`、`Apple.list`、`HK_Broker.list`、`HSBC_HK.list`、`HK_Banks_Direct.list` 等规则集来自上游 [LingJingMaster/Shadowrocket-Rules](https://github.com/LingJingMaster/Shadowrocket-Rules)
- TikTok 规则集来自 blackmatrix7 `Shadowrocket/TikTok/TikTok.list`
- 日本AV 使用上游 RULE-SET：mediacvg/ytnfst `jpav.list`（DMM / FANZA、duja、syukou-club、x-diablo 等），自动随上游更新；另有 `conf` 内手动补充的厂商 `DOMAIN-SUFFIX` 规则（S1、Moodyz、IdeaPocket、SOD、Prestige、Faleno、Caribbeancom、Heyzo 等），来源：andyyippro/rule `JapanSites.list`、eric24lu/Surge `NSFW.list`、Travis0234/rules4mihomo `nsfw.list`；未包含聚合 / 盗版站
- missav 使用上游 RULE-SET：sub-kek/shadowrocket-lists `shadowrocket/missav.list`；`javdb`、`javbus` 等聚合站仍为 `conf` 内的 `DOMAIN-SUFFIX` 规则

## Star History

[![Star History Chart](https://api.star-history.com/chart?repos=lingjingmaster/shadowrocket-rules&type=date&legend=top-left)](https://www.star-history.com/?repos=lingjingmaster%2Fshadowrocket-rules&type=date&legend=top-left)

## License

MIT
