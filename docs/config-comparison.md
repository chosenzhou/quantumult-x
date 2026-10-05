# 原版与当前配置对比

核对日期：2026-10-05。对象为本地 `original-rules.conf`、`quantumult-x.conf` 和 `rules/custom.list`。本文不记录个人订阅地址、证书或口令。

`original-rules.md` 本来就是纯配置，已改名为 `original-rules.conf`；改名前后内容逐字节一致，原版行为没有改变。

## 已核实的差异

| 项目 | 原版 | 当前配置 | 实际影响 |
| --- | --- | --- | --- |
| 节点、策略组、测速 | 原有配置 | 行为相同 | 没有性能提升的测试依据；遵循用户要求保留 |
| DNS、UDP 回退 | 两个国内 DNS；不支持 UDP 时拒绝 | 相同 | 两版共用这些取舍 |
| 自定义分流 | 主要写在本地配置 | 迁入仓库的 custom.list | 后续可通过远程资源更新；增加对 GitHub 资源获取的依赖 |
| 分流更新间隔 | -1 | 86400 秒 | 从关闭自动同步改为每日自动同步；客户端缓存不等于首次下载保证 |
| AI 上游 | OpenAI、Gemini、Claude、Copilot | 保留四套，新增 Civitai | 四套并非当前版本新增；Civitai 本来就在 Global，新版主要将它改分到 AI |
| AI 自定义补充 | 原有自定义域名 | 保留补充，新增 claude.com；移除已由上游覆盖的 Copilot 和 generativelanguage 条目 | 相应上游成功加载时，移除的重复项仍有覆盖 |
| 国际服务补充 | Global | GitHub、Telegram、YouTube，再用 Global | 可补充 Global 未列出的模式；多数内容重合，不能按列表数量判断效果 |
| 本地 IP | 原有 IPv4 局域网规则 | 补充 IPv4 链路本地、组播、有限广播，以及 IPv6 本地地址和组播规则 | 本地与组播请求明确直连 |
| su8.codes | 明确 direct | 没有专门规则 | 用户确认网站不再使用，当前规则不恢复该条目 |
| 广告复写 | enabled=true | 已删除该订阅 | 减少大范围 HTTPS 复写；也失去这套 URL 级去广告能力 |
| 国内与最终兜底 | 本地 geoip 和 final | 同样留在本地 | 不依赖已删除的 fallback.list |

原版有两处注释与行为不一致：声称普通 Bing 不走 AI，但已经启用 Copilot 全套；声称广告复写默认关闭，但实际为 `enabled=true`。

## 结论与限制

对“从 GitHub 维护规则、定期同步、保留覆盖优先 AI、不用大范围广告复写”的需求，当前配置更合适。原版把个人规则放在一个文件中，初次配置的外部依赖更少。没有设备端延迟、解析结果或应用可用性对照，无法认定任一版本更快。

两版都包含共享域名与 ASN 的 AI 分流。当前选择已由用户确认，本文不把它当作待修复错误。OpenAI 与 Copilot 的公共服务规则，以及 Gemini 的 apis.google.com / colab 匹配，都可能影响其他应用。

## 本轮已确认并落实的决定

- `su8.codes` 不再使用，当前配置和自定义列表继续不包含其专门直连规则；原版对照文件保留历史内容。
- 补齐本地地址直连：IPv4 保留私有地址、回环和链路本地规则，将组播扩大为 `224.0.0.0/4`，增加有限广播 `255.255.255.255/32`；IPv6 增加回环 `::1/128`、唯一本地 `fc00::/7`、链路本地 `fe80::/10` 和组播 `ff00::/8`。
- 运营商共享地址 `100.64.0.0/10` 没有作为局域网整段加入。
- DNS 保持目前设置，优先保持现有网络兼容性，不新增 `no-system`、DoH 或 DoQ。
- 节点组英文筛选、测速地址、覆盖优先 AI 分流和最终兜底选择保持已确认的设置。

## 后续有实际问题时再完善

- 如果 AI 会话出现出口变动问题，再评估单独的固定出口选项；测速最优并不保证服务可用或地区合适。
- 如果出现 DNS 解析问题，再核对系统 DNS 与公共 DNS 的实际结果，并考虑加密解析和局域网名称解析的取舍。
- 为新增 AI 服务补规则前，检查真实请求记录，区分遗漏域名、出口可用性与账号/地区限制。
- 远程 custom.list 初次获取失败时，自定义补充可能不可用；设备端核对资源加载和缓存更新结果。
- 自定义规则从本地迁为远程后，核对客户端的分流匹配优化、资源插入状态与真实命中记录。官方示例没有说明这些组合下的完整匹配顺序，不能只按配置文件中各段的位置判断优先级。

## 核对来源

- [Quantumult X 官方示例](https://github.com/crossutility/Quantumult-X/blob/master/sample.conf)：自动同步间隔、策略测速、DNS 与系统 DNS 的行为。
- [blackmatrix7 OpenAI](https://github.com/blackmatrix7/ios_rule_script/blob/master/rule/QuantumultX/OpenAI/OpenAI.list)、[Copilot](https://github.com/blackmatrix7/ios_rule_script/blob/master/rule/QuantumultX/Copilot/Copilot.list)、[Gemini](https://github.com/blackmatrix7/ios_rule_script/blob/master/rule/QuantumultX/Gemini/Gemini.list)：共享依赖与宽泛匹配。
- [RFC 4291](https://www.rfc-editor.org/rfc/rfc4291.html)、[RFC 4193](https://www.rfc-editor.org/rfc/rfc4193.html)：IPv6 回环、链路本地、组播和唯一本地地址范围。

以上结论为文件和官方资料核对结果，尚未在 Quantumult X 客户端做加载与流量实测。
