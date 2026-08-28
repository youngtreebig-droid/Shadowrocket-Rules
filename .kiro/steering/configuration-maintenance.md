---
inclusion: always
---

# Configuration maintenance

- Treat `Shadowrocket.conf`, `Clash_Merge.yaml`, and generated `Clash_Script.js` as paired configuration sources. When a routing rule, proxy group, default selection, or rule-provider changes in one, make the equivalent change in the other unless the user explicitly requests a client-specific difference.
- Preserve equivalent rule priority between the two clients. A Shadowrocket `RULE-SET` needs a corresponding Clash `rule-provider` and `RULE-SET` rule.
- Keep all user-selectable proxy groups as `select` unless the user explicitly requests another type. Map Shadowrocket `policy-select-name` to Clash `default-selected`.
- The default selection for `Ⓜ️ 微软服务`, `🍏 苹果服务`, `🍎 苹果推送`, `📈 券商服务`, `🏠 私有网络`, and `🔒 国内服务` is `DIRECT`. Do not change these defaults without an explicit request.
- Keep domain-based domestic rules ahead of `GEOIP,CN`, the global rule set, and the final fallback. This preserves domestic routing when the client uses Fake-IP.
- `Clash_Merge.yaml` is replacement-mode only. For subscriptions that define their own proxy groups or rules, maintain `Clash_Script.js` as the recommended Clash Verge entry point so their groups and non-terminal rules are preserved.
- After changing `Clash_Merge.yaml`, run `ruby scripts/generate_clash_script.rb` and commit `Clash_Script.js`; do not edit the generated script manually.
- When editing `MissAV.list`, `AI.list`, `Apple.list`, `ApplePush.list`, `Google.list`, `HSBC_HK.list`, `HK_Banks_Direct.list`, or `HK_Broker.list`, run `python3 scripts/generate_clash_rule_providers.py` and commit any changed files under `Clash/rules/`.
- Validate `Clash_Merge.yaml` as YAML and verify its proxy-group and rule-provider references after changes.
- Never commit subscription URLs, tokens, credentials, or individual proxy node definitions. The public repository contains rules only.
- `README.md` is intentionally limited to the configuration QR image. Do not add explanatory text unless the user explicitly asks.
