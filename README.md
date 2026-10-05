# Quantumult X 自定义分流

## 配置方式

仓库中的 `quantumult-x.conf` 是公开模板；个人订阅地址和 MITM 证书只保存在本地配置中。自定义规则维护在 [`rules/custom.list`](rules/custom.list)，推送到 `main` 后由 Quantumult X 每 86400 秒更新一次。

当前采用覆盖优先的 AI 分流：OpenAI、Gemini、Claude、Copilot、Civitai 使用 blackmatrix7 的原生规则；其中 OpenAI / Copilot 可能把共享域名和普通 Bing 分到 `AI`。`custom.list` 只补充这些列表未覆盖的 AI 域名和国内 AI 直连。

`[filter_remote]` 的加载顺序如下：

```ini
[filter_remote]
https://raw.githubusercontent.com/chosenzhou/quantumult-x/main/rules/custom.list, tag=自定义分流, update-interval=86400, opt-parser=false, enabled=true

https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/AdvertisingLite/AdvertisingLite.list, tag=广告拦截, force-policy=reject, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Hijacking/Hijacking.list, tag=运营商劫持, force-policy=reject, update-interval=86400, opt-parser=false, enabled=true

https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/OpenAI/OpenAI.list, tag=OpenAI, force-policy=AI, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Gemini/Gemini.list, tag=Gemini, force-policy=AI, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Claude/Claude.list, tag=Claude, force-policy=AI, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Copilot/Copilot.list, tag=Copilot, force-policy=AI, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Civitai/Civitai.list, tag=Civitai, force-policy=AI, update-interval=86400, opt-parser=false, enabled=true

https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Crypto/Crypto.list, tag=数字货币, force-policy=数字货币, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Apple/Apple.list, tag=苹果服务, force-policy=苹果服务, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Microsoft/Microsoft.list, tag=微软服务, force-policy=微软服务, update-interval=86400, opt-parser=false, enabled=true

https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/China/China.list, tag=国内网站, force-policy=direct, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/ChinaIPs/ChinaIPs.list, tag=国内IP池, force-policy=direct, update-interval=86400, opt-parser=false, enabled=true

https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/GitHub/GitHub.list, tag=GitHub, force-policy=全球加速, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Telegram/Telegram.list, tag=Telegram, force-policy=全球加速, update-interval=86400, opt-parser=false, enabled=true
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/YouTube/YouTube.list, tag=YouTube, force-policy=全球加速, update-interval=86400, opt-parser=false, enabled=true

https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Global/Global.list, tag=国际网站, force-policy=全球加速, update-interval=86400, opt-parser=false, enabled=true
```

`geoip, cn, direct` 和 `final, 最终兜底` 保留在 `quantumult-x.conf` 的 `[filter_local]` 末尾。`最终兜底` 策略组仍可选 `全球加速` 或 `direct`；`fallback.list` 已删除。
