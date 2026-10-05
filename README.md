# Quantumult X 自定义分流

这个仓库存放 Quantumult X 的自定义远程规则。以后修改 [`rules/custom.list`](rules/custom.list) 并推送到 `main`，Quantumult X 会按资源更新间隔获取新版本。`rules/fallback.list` 存放最后执行的国内 IP 与最终兜底规则。

## 在现有配置中只需改一次

1. 清空原有 `[filter_local]` 的规则内容，保留空的 `[filter_local]` 段。
2. 用下面的内容替换原有 `[filter_remote]` 段。自定义服务规则放在前面，兜底规则放在最后；`force-policy` 不用于自定义文件，因为里面同时有 `AI` 和 `direct`。
3. 在 Quantumult X 中更新一次远程资源。此后规则文件按 `update-interval=86400`（秒）自动更新；刚推送完如需立即生效，可手动更新远程资源。

```ini
[filter_remote]
https://raw.githubusercontent.com/chosenzhou/quantumult-x/main/rules/custom.list, tag=自定义分流, update-interval=86400, opt-parser=false, enabled=true

https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/AdvertisingLite/AdvertisingLite.list, tag=广告拦截, force-policy=reject, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Hijacking/Hijacking.list, tag=运营商劫持, force-policy=reject, update-interval=86400, opt-parser=false, enabled=true

https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Claude/Claude.list, tag=Claude, force-policy=AI, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Crypto/Crypto.list, tag=数字货币, force-policy=数字货币, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Apple/Apple.list, tag=苹果服务, force-policy=苹果服务, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Microsoft/Microsoft.list, tag=微软服务, force-policy=微软服务, update-interval=86400, opt-parser=false, enabled=true

https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/China/China.list, tag=国内网站, force-policy=direct, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/ChinaIPs/ChinaIPs.list, tag=国内IP池, force-policy=direct, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Global/Global.list, tag=国际网站, force-policy=全球加速, update-interval=86400, opt-parser=false, enabled=true

https://raw.githubusercontent.com/chosenzhou/quantumult-x/main/rules/fallback.list, tag=国内IP与最终兜底, update-interval=86400, opt-parser=false, enabled=true

[filter_local]
```

OpenAI、Gemini、Copilot 三个上游合集有共享服务域名或较宽的匹配项，本配置改为由 `rules/custom.list` 精准指定主要入口。这样普通 Bing、Stripe、Auth0、Sentry、Cloudflare 等不会因为 AI 规则而整体进入 `AI`。如果某项服务无法正常工作，先在 Quantumult X 的活动记录中确认实际请求域名，再把必要的专属域名加入 `rules/custom.list`。

## 其他配置检查

- 原配置中的 `rewrite_remote` 注释称去广告复写默认关闭，但实际为 `enabled=true`。该规则含大量 MITM 主机名；如果你原意是关闭，请改为 `enabled=false`。启用时应确认自己需要哪些复写功能。
- 原配置所有远程规则都用了 `update-interval=-1`，表示关闭自动更新。仅更新 GitHub 文件而不改变这一项，不会自动同步到设备。
- 香港、日本、美国节点组只匹配英文 `Hong Kong`、`Japan`、`United States`。如果订阅节点名称为中文或 HK/JP/US 缩写，对应策略组可能为空；按实际节点标签调整正则。
- `server_check_url=http://www.qualcomm.cn/generate_204` 建议在设备上确认确实稳定返回 204；测速结果取决于这个地址的可达性。
- 本仓库不存放机场订阅地址、MitM 证书、密码或私钥。
