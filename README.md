> 如需Clash版本 → [请点击此处](https://github.com/LingJingMaster/Clash-Rules)

---

# 小火箭 Shadowrocket Config

一份开箱即用的 小火箭 Shadowrocket 规则配置
- 支持前沿的苹果智能（Apple Intelligence）
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
| 3 | 微信、1Password 等精确直连；国内 AI（通义、智谱、Kimi、豆包、DeepSeek 等，VPSDance `cn.list`）；抖音 / 字节的风控、网络调度等例外放行 | DIRECT（固定） |
| 4 | 🤖 AI 服务（大陆需代理的海外 AI：ChatGPT / Codex、Claude / Claude Code、Grok / Grok Bot、Kiro、Notion、Copilot、Cursor、Windsurf、Perplexity、Mistral、Meta AI、Hugging Face、OpenRouter、Midjourney、Suno、ElevenLabs、苹果智能等百余家服务；Google 与 Gemini 不在此组，见下方 🔍 谷歌与 Gemini） | 日本节点 |
| 5 | 📶 测速服务（Speedtest / Ookla / Fast.com / Cloudflare Speed，blackmatrix7 `Speedtest.list` + `ookla.com`） | PROXY（首页当前选中的节点），可在分组里切换 |
| 6 | 🛑 广告 / 追踪拦截（Loyalsoldier `reject` 约 19 万条 + blackmatrix7 `BlockHttpDNS`；Netflix / Spotify 风控与实验域名例外放行） | REJECT |
| 7 | 📧 邮件服务（IMAP / POP3 / SMTP） | 香港节点，可切换 |
| 8 | 📹 油管视频（含 YouTube 翻译 API；放在谷歌之前，否则 googlevideo.com 视频流会被 `Google.list` 截走） | 香港节点 |
| 9 | 🔍 谷歌与 Gemini（`Google.list` + VPSDance `google-ai.list`；Google 搜索、登录、自家统计与 Gemini / AI Studio / NotebookLM / Antigravity 等同走此组，出口一致） | 日本节点 |
| 10 | 🎵 TikTok（抖音国际版 / CapCut 等） | 日本节点 |
| 11 | 🎬 Netflix（blackmatrix7 `Netflix.list` + Netflix 自己的 AWS 地址；修正了上游的过宽规则，AWS 改走 🤖 AI 服务、OneTrust Cookie 弹窗改走 PROXY） | 香港节点 |
| 12 | 🎧 Spotify（blackmatrix7 `Spotify.list`） | 香港节点 |
| 13 | 🔞 日本AV（DMM / FANZA、S1、Moodyz 等片商） | 日本节点 |
| 14 | 💋 missav（missav 及 JAV 聚合 / 在线观看站） | 香港节点 |
| 15 | 哔哩哔哩 | DIRECT（固定） |
| 16 | 📲 电报消息、🐱 代码托管（GitHub、GitLab、Atlassian） | PROXY（固定） |
| 17 | Ⓜ️ 微软服务 | 香港节点 |
| 18 | 🏦 汇丰香港 / 香港银行 | DIRECT（固定） |
| 19 | 🍎 苹果推送、🍏 苹果服务 | DIRECT（固定） |
| 20 | 🇨🇳 国内域名（blackmatrix7 `ChinaMax_Domain`） | DIRECT（固定） |
| 21 | 🌐 境外域名（blackmatrix7 `Global_Domain`、`GlobalMedia_Domain`） | PROXY（固定） |
| 22 | 含 IP 的规则：国内 IP / 关键字（blackmatrix7 `ChinaMax`）→ DIRECT；境外 IP / 关键字（blackmatrix7 `Global`、`GlobalMedia`，Loyalsoldier `telegramcidr`）→ PROXY | 见左 |
| 23 | GEOIP CN | DIRECT |
| 24 | 🐟 漏网之鱼（所有列表都没匹配到的流量） | 香港节点 |

规则从上往下匹配，命中即停。排序原则：
- 精确、小范围的规则放前面，大而全的国内 / 境外列表放后面，避免大列表抢走需要单独分流的服务
- 只匹配域名的规则放前面，含 IP 的规则（`IP-CIDR`、`GEOIP`）放最后。匹配 IP 前必须先做 DNS 解析，放在前面会让每个请求都先等解析，变慢
- DNS：`dns-server` 用阿里和腾讯的加密 DNS（DoH）。它只用来查直连的网站，以及没被任何列表认出、要查到 IP 才能用 `GEOIP,CN` 判断的网站；走代理的网站由代理服务器在海外查，不经过它，所以不受 DNS 污染影响。用国内 DNS，国内网站拿到的是国内服务器地址，节点断了也能查。主 DNS 查询失败时才用 `fallback-dns-server`（Google，经代理）
- 局域网规则的 IP 部分带 `no-resolve`，不会触发解析，所以可以放最前
- 你自己的分组（AI、油管、TikTok、Netflix、Spotify、日本AV、missav、微软等）全部排在 Global 等境外代理列表之前，命中分组就按分组选的节点走，不会被改成 PROXY
- 谷歌全系（`Google.list`）与 Gemini 单独走 🔍 谷歌与 Gemini（默认日本节点；原因：韩国节点被 Google 误判为香港，Gemini 不可用，且 ChatGPT / Claude 等仍可继续用韩国），排在 📹 油管视频之后、国内直连 `ChinaMax` 之前：油管视频流仍走 📹 油管视频，而 Google 搜索、登录（`accounts.youtube.com`、`apis.google.com`）、前端静态资源（`gstatic`、`recaptcha`）和自家统计（Firebase、Crashlytics、`gvt2.com` 等）全部与 Gemini 走同一个 🔍 谷歌与 Gemini 出口，既保证搜索里的 AI 概览 / AI 模式可用，又避免触发 Google "验证设备"；谷歌广告仍然拦截
- Speedtest 测速规则放在广告拦截和国内直连之前，走 📶 测速服务（默认 PROXY，即首页当前选中的节点），避免 `sgtm.ookla.com` 被拦截或测速连接在直连与代理之间分裂报错
- 抖音 / 字节跳动国内服务（blackmatrix7 `DouYin`、`ByteDance`）固定直连，并放在 🎵 TikTok 之前：blackmatrix7 `TikTok.list` 收录了抖音主域名 `snssdk.com`（抖音接口、支付、验证码、设备风控都在下面），不先直连会被当成 TikTok 走美国节点。字节的安全风控（`mssdk.bytedance.com`、`xlog.snssdk.com`）和网络调度（`tnc3-alisc1.bytedance.com`）不拦截；字节的广告和统计域名仍然拦截
- TikTok 自家域名（v2fly `tiktok` 和 `isnssdk.com`、`capcutapi.com` 等）放在广告拦截之前，统一走 🎵 TikTok：广告拦截列表会拦 TikTok 的私信、长连接、网络调度和安全 SDK，导致好友页、私信一直加载；TikTok 信息流广告和视频走同一组接口，按域名本来就拦不掉。TikTok 网页追踪像素（`analytics.tiktok.com`）、广告后台 `ads.tiktok.com` 和 Pangle 广告联盟继续拦截
- 广告拦截放在 🤖 AI 服务之后、谷歌和国内直连等大列表之前：AI 依赖的统计域名（statsig、datadog 等）不会被误杀，doubleclick、admob 等广告域名又能先被拦截


## 默认策略

| 服务 | 默认策略 | 可选策略 |
|------|----------|----------|
| 📧 邮件服务 | 🇭🇰 香港节点 | 节点选择、PROXY、DIRECT、REJECT、其他地区节点 |
| 🎬 Netflix | 🇭🇰 香港节点 | 节点选择、PROXY、DIRECT、REJECT、其他地区节点 |
| 🎧 Spotify | 🇭🇰 香港节点 | 节点选择、PROXY、DIRECT、REJECT、其他地区节点 |
| 🤖 AI 服务（含 Notion、AWS） | 🇰🇷 韩国节点 | 节点选择、PROXY、DIRECT、REJECT、其他地区节点 |
| 🔍 谷歌与 Gemini | 🇯🇵 日本节点 | 节点选择、PROXY、DIRECT、REJECT、其他地区节点 |
| 🎵 TikTok | 🇯🇵 日本节点 | 节点选择、PROXY、DIRECT、REJECT、其他地区节点 |
| 🔞 日本AV | 🇯🇵 日本节点 | PROXY、DIRECT、REJECT、节点选择、其他地区节点 |
| 💋 missav | 🇭🇰 香港节点 | PROXY、DIRECT、REJECT、节点选择、其他地区节点 |
| 📹 油管视频 | 🇭🇰 香港节点 | 节点选择、PROXY、DIRECT、REJECT、其他地区节点 |
| Ⓜ️ 微软服务 | 🇭🇰 香港节点 | 节点选择、PROXY、DIRECT、REJECT、其他地区节点 |
| 📶 测速服务 | PROXY（首页当前选中的节点） | 节点选择、DIRECT、REJECT、所有地区节点 |
| 🐟 漏网之鱼 | 🇭🇰 香港节点 | PROXY、节点选择、DIRECT、REJECT、其他地区节点 |

- 🚀 节点选择 默认 PROXY（即首页当前选中的节点）
- 🇭🇰 香港节点 是自动测速组（url-test），只收录名字里带 `JZ` 的香港节点，每 10 分钟测一次延迟，自动用最快的节点；另一个节点快 50ms 以上才切换，避免来回跳。名字里没有 `JZ` 的香港节点不进任何地区组，只能在首页直接选
- 其他地区分组都是手动选择组（select），不测速、不做倍率过滤，可在组内改选。🇯🇵 日本节点 默认为 `Tokyo2-VLESS`；🇹🇼 台湾节点 / 🇰🇷 韩国节点 / 🇸🇬 新加坡节点 / 🇺🇸 美国节点 / 🌏 亚洲其他节点 / 🌐 全球其他节点 默认是组内第一个节点；🇺🇸 美国节点 排除了不再使用的 `美国-166.0.26.50` 节点
- 每个策略组都能直接选所有地区节点：🇭🇰 香港、🇹🇼 台湾、🇯🇵 日本、🇰🇷 韩国、🇸🇬 新加坡、🇺🇸 美国、🌏 亚洲其他、🌐 全球其他，以及 JZ 其他节点
- 地区分组按节点名里的国旗和中英文地名匹配。🌏 亚洲其他节点：以上地区之外的亚洲节点，包括澳门、东南亚、南亚、中亚、中东和土耳其；🌐 全球其他节点：剩下的所有节点，不含机场订阅里“剩余流量”“套餐到期”这类信息条目
- 固定策略（不在策略组中，直接写入规则）：
- 📲 电报消息、🐱 代码托管、🌐 其他境外服务（Twitter / X、Facebook、Instagram、Reddit、Wikipedia、Discord、Disney+、Twitch 等，不在分组里的都算） → PROXY，即首页当前选中的节点
- OneTrust Cookie 弹窗（`cookielaw.org`、`onetrust.com`） → PROXY
- 🍎 苹果推送、🍏 苹果服务、🏦 汇丰香港、🏦 香港银行、🔒 国内服务、🏠 私有网络 → DIRECT
- 港股券商不单独处理，按通用规则分流
- 🔐 1Password → DIRECT（上游 EliDoves/Proxy `1Password.list` + 官方文档补充域名；1Password 无公开固定 IP，按域名匹配）
- 🧱 DNS 防泄露、WebRTC 防泄露、🛑 广告 / 追踪（Loyalsoldier `reject`） → REJECT







## 规则集来源

- [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) — 主要规则集；国内直连用 `ChinaMax`（含 China、ChinaIPs 等全部国内规则）
- [Loyalsoldier/surge-rules](https://github.com/Loyalsoldier/surge-rules) — `reject`（广告 / 追踪拦截）、`private`（局域网域名），每日构建。国内直连只用 blackmatrix7 ChinaMax：其说明要求引用 ChinaMax 后不再叠加其他国内规则，所以没有引用 Loyalsoldier `direct`、`cncidr`
- [VPSDance/ai-proxy-rules](https://github.com/VPSDance/ai-proxy-rules) `cn.list` — 国内 AI 直连
- 境外代理：blackmatrix7 `Global`、`GlobalMedia`（流媒体），Loyalsoldier `telegramcidr`，统一走 PROXY；放在 ChinaMax 之后，和国内列表重合的域名优先直连。Loyalsoldier `proxy` 有 2.7 万条，但只比 Global 多约 160 条，不再引用
- 例外：B 站国际版 `bilibili.tv`、App Store 短链接 `appsto.re` 在被墙列表里，但上游 BiliBili、Apple 列表把它们算成直连，配置里单独让它们走 PROXY
- [iab0x00/ProxyRules](https://github.com/iab0x00/ProxyRules) — AI 服务补充规则
- 🤖 AI 服务（规则整体放在 `Microsoft.list` 之前，避免 Copilot 被 Ⓜ️ 微软服务提前匹配；Google 全系与 Gemini 不在本组，单独走 🔍 谷歌与 Gemini；国内 AI、字节的海外 AI 产品（Trae、MarsCode、Coze、Cici / Dola，走 🎵 TikTok）、国内公司 AI 的海外版（Qoder、Kimi、MiniMax，走 PROXY）不在此组）：
  - [VPSDance/ai-proxy-rules](https://github.com/VPSDance/ai-proxy-rules) `rules/shadowrocket/global.list`：海外 AI 汇总，百余家厂商，每日同步
  - [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community) `geosite:category-ai-!cn`，Shadowrocket 格式由 [ddranic/ios-proxy-rules](https://github.com/ddranic/ios-proxy-rules) 每日转换
  - [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) `Copilot`（`OpenAI`、`Claude`、`Gemini` 三个列表已被其他列表完全覆盖，不再引用）
  - 上游 LingJingMaster `AI.list`（苹果智能等）
  - Notion：blackmatrix7 `Notion.list`、v2fly `geosite:notion`（ddranic 转换）。Notion 的附件存在 AWS S3，跟随 AWS 同样走本组
  - AWS：`amazonaws.com` 全部走本组（规则放在广告拦截之后、`Netflix.list` 之前）。例外：Netflix 自己的 AWS 地址走 🎬 Netflix，Grok Bot 附件桶直连，AWS 上的广告追踪地址仍拦截；AWS 中国区（`amazonaws.com.cn`）不受影响
  - `conf` 内手动补充上游未收录的域名：Kiro 的 `kiro.aws.dev`、Cognito 登录端点（来源 [Kiro 防火墙文档](https://kiro.dev/docs/privacy-and-security/firewalls/)）；Grok 的 `grokusercontent.com`、`spacex.ai`（来源 [xAI 登录域名文档](https://docs.x.ai/x-subscription/sign-in-domains)）；以及 Le Chat、Krea、Higgsfield、Luma、Black Forest Labs、Tensor.Art、Warp、TypingMind、ChatPDF、Elicit、Consensus
  - 注意：上游为保证 AI 服务完整可用，也收录了少量通用域名（如 `storage.googleapis.com`、`intercom.io`，以及含 `sentry`、`datadog` 关键字的域名），这些流量同样会走 🤖 AI 服务
- `Mail.list`、`Apple.list`、`HSBC_HK.list`、`HK_Banks_Direct.list` 等规则集来自上游 [LingJingMaster/Shadowrocket-Rules](https://github.com/LingJingMaster/Shadowrocket-Rules)
- TikTok 规则集来自 blackmatrix7 `Shadowrocket/TikTok/TikTok.list`
- Netflix 使用上游 RULE-SET：blackmatrix7 `Netflix.list`。它收录了 `us-west-2.amazonaws.com`（整个 AWS 美西区域）和 `cookielaw.org`、`onetrust.com`（OneTrust Cookie 弹窗，很多网站都在用），范围过大；`conf` 在它前面补了 Netflix 自己的 AWS 负载均衡地址（来源 v2fly `geosite:netflix`），再让 AWS 全部地址走 🤖 AI 服务、OneTrust 走 PROXY
- 日本AV 全部是 `conf` 内的 `DOMAIN-SUFFIX` 规则：DMM / FANZA、duja、syukou-club、x-diablo 来自 mediacvg/ytnfst `jpav.list`（不再引用整份列表，因为它的 `DOMAIN-KEYWORD,dmm` 会把域名里带 dmm 的国内网站也带进日本AV 分组），以及手动补充的厂商（S1、Moodyz、IdeaPocket、SOD、Prestige、Faleno、Caribbeancom、Heyzo 等），来源：andyyippro/rule `JapanSites.list`、eric24lu/Surge `NSFW.list`、Travis0234/rules4mihomo `nsfw.list`；未包含聚合 / 盗版站
- missav 使用上游 RULE-SET：sub-kek/shadowrocket-lists `shadowrocket/missav.list`；`javdb`、`javbus` 等聚合站仍为 `conf` 内的 `DOMAIN-SUFFIX` 规则

## Star History

[![Star History Chart](https://api.star-history.com/chart?repos=lingjingmaster/shadowrocket-rules&type=date&legend=top-left)](https://www.star-history.com/?repos=lingjingmaster%2Fshadowrocket-rules&type=date&legend=top-left)

## License

MIT
