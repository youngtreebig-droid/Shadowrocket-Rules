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
| 1 | 📧 邮件服务（IMAP / POP3 / SMTP） | PROXY，可切换 DIRECT 或地区节点 |
| 2 | 🔍 谷歌服务（含 Gemini） | 日本节点，可手动切香港、美国节点 |
| 3 | 🤖 AI 服务（ChatGPT、Claude、苹果智能等） | 美国节点 |
| 4 | 📹 油管视频（含 YouTube 翻译 API） | 节点选择 |
| 5 | 🎵 TikTok（抖音国际版 / CapCut 等） | 美国节点 |
| 6 | 📲 电报消息 | PROXY（固定） |
| 7 | 🐱 代码托管（GitHub、GitLab、Atlassian） | PROXY（固定） |
| 8 | Ⓜ️ 微软服务 | 节点选择 |
| 9 | 🍎 苹果推送 | DIRECT |
| 10 | 🍏 苹果服务 | DIRECT |
| 11 | 🏦 汇丰香港 / 香港银行 / 📈 券商服务 | DIRECT（固定） |
| 12 | 🔒 哔哩哔哩 / 国内服务 / 🏠 私有网络 | DIRECT（固定） |
| 13 | 🐟 漏网之鱼（兜底，含非中国境外流量） | PROXY |
| 14 | GEOIP CN | DIRECT |


## 默认策略

| 服务 | 默认策略 | 可选策略 |
|------|----------|----------|
| 📧 邮件服务 | PROXY | DIRECT、节点选择、美国 / 日本 / 香港节点 |
| 🔍 谷歌服务 | 🇯🇵 日本节点 | 🇭🇰 香港节点、🇺🇸 美国节点、节点选择、PROXY、DIRECT |
| 🤖 AI 服务 | 🇺🇸 美国节点 | 节点选择、PROXY、DIRECT、日本节点、香港节点 |
| 🎵 TikTok | 🇺🇸 美国节点 | 节点选择、PROXY、DIRECT、日本节点、香港节点 |
| 📹 油管视频 | 🚀 节点选择 | PROXY、DIRECT、美国 / 日本 / 香港节点 |
| Ⓜ️ 微软服务 | 🚀 节点选择 | PROXY、DIRECT、美国 / 日本 / 香港节点 |
| 🍎 苹果推送 | DIRECT | 节点选择、PROXY、美国 / 日本 / 香港节点 |
| 🍏 苹果服务 | DIRECT | 节点选择、PROXY、美国 / 日本 / 香港节点 |
| 🐟 漏网之鱼 | PROXY | 节点选择、DIRECT、美国 / 日本 / 香港节点 |

- 🌐 其他节点 / 🇭🇰 / 🇹🇼 / 🇯🇵 / 🇺🇸 自动组默认过滤掉倍率 ≥ 1 的节点（节点名需带 `[倍率]`，如 `[6.0]`），需要时可在「节点选择」里手动选择
- 固定策略（不在策略组中，直接写入规则）：
- 📲 电报消息、🐱 代码托管 → PROXY
- 🏦 汇丰香港、🏦 香港银行、📈 券商服务、🔒 国内服务、🏠 私有网络 → DIRECT
- 🧱 DNS 防泄露、WebRTC 防泄露 → REJECT







## 规则集来源

- [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) — 主要规则集
- [iab0x00/ProxyRules](https://github.com/iab0x00/ProxyRules) — AI 服务补充规则
- `Mail.list`、`Apple.list`、`HK_Broker.list`、`HSBC_HK.list`、`HK_Banks_Direct.list` 等规则集来自上游 [LingJingMaster/Shadowrocket-Rules](https://github.com/LingJingMaster/Shadowrocket-Rules)
- TikTok 规则集来自 blackmatrix7 `Shadowrocket/TikTok/TikTok.list`

## Star History

[![Star History Chart](https://api.star-history.com/chart?repos=lingjingmaster/shadowrocket-rules&type=date&legend=top-left)](https://www.star-history.com/?repos=lingjingmaster%2Fshadowrocket-rules&type=date&legend=top-left)

## License

MIT
