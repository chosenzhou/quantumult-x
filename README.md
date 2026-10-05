# Quantumult X 配置

本仓库保存一份可由自己控制的 Quantumult X 配置。主配置来自 [curtinp118/QuantumultX](https://github.com/curtinp118/QuantumultX)，所引用的 22 份 blackmatrix7 分流规则已复制到本仓库。个人修改集中在 [`rules/Custom.list`](rules/Custom.list)。

## 使用

在 Quantumult X 的「配置文件 → 下载」中填入：

```text
https://raw.githubusercontent.com/chosenzhou/quantumult-x/main/profile/QX_Config.conf
```

导入前请备份当前配置。主配置中的上游公益节点订阅已移除；请在 Quantumult X 本地添加自己的节点订阅，并确认 `AI` 等策略组选中了实际可用的节点。节点订阅 URL、节点密码、Cookie、MITM 证书和口令不要提交到这个公开仓库。

以后要补充或覆盖分流，只编辑 [`rules/Custom.list`](rules/Custom.list)。该文件放在其他远程分流规则之前，每条规则写明策略组，例如 `HOST,example.com,AI` 或 `HOST-SUFFIX,example.org,direct`。保存到 GitHub 后，在 Quantumult X 中更新名为 `Custom` 的远程规则资源。

## 来源与更新

- `profile/QX_Config.conf`、`rules/AI.list`、`rules/AppleIntelligence.list`、`rules/dandan.list` 和 `icons/set.png` 来自 curtinp118/QuantumultX 的提交 `35d2d3bbaca02a92b94104fe12de098c727f3a7a`。保留了原项目的 [MIT 许可证](LICENSES/curtinp118-MIT.txt)。
- `vendor/blackmatrix7/rule/QuantumultX/` 中的 22 份规则来自 blackmatrix7/ios_rule_script 的提交 `5a61490ab88ddaff4e9dbd7740b881d75157a49f`。其 [`LICENSE`](vendor/blackmatrix7/LICENSE) 为 GPL-2.0。这里只复制了主配置实际引用的规则，并非整个规则库。
- `vendor/TG-Twilight/`、`vendor/fmz200/`、`vendor/chavyleung/`、`vendor/app2smile/`、`vendor/xream/` 保存了主配置直接引用、且有明确再分发许可证的第三方资源，以及 Spotify 和 BoxJS 复写直接调用的脚本。每个来源目录保留了各自的许可证。固定来源提交分别为 `99b020c21853cbed56b90456399dad6bde842309`、`5d5f63fcf98bc69d5f8f1b1bae6f86a01ee4bb97`、`2c30c299e3ef808b787054d256250a4adfb93b34`、`df6366a7024e0b3f0aa3510c5b791eea6f3cba89`、`b911f31444206ea8e4110f7de7ca4f4530c9221f`。
- 这些是固定快照。上游更新不会自动写入本仓库；上游删除后，本仓库中的文件仍可使用。

主配置仍引用以下未明确授权再分发的文件，原地址失效时对应功能可能受影响：

| 来源 | 配置中的用途 |
| --- | --- |
| KOP-XIAO/QuantumultX | 资源解析器、IP 查询、流媒体查询 |
| Koolson/Qure、Toperlock/Quantumult | 策略组图标 |
| ZenmoFeiShi/Qx | YouTube 字幕与去广告复写 |
| CC13594759/QuantumultX、RavelloH Gist、ddgksf2013.top | 节点检测脚本 |

BoxJS 脚本内部还引用更多外部资源；DNS、网络检测和 IP 查询等在线服务也无法作为静态文件镜像。curtinp118 的独立 `Scripthub` 仓库不在上述快照中。启用远程复写及 MITM 前，请检查相应资源的用途。
