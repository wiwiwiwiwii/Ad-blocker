# Ad-blocker

个人使用的 Stash（iOS / iPadOS）去广告补充覆写。

本仓库只维护 `Extra-AdBlock.stoverride`：针对常用 App、且未被 Shiina 模块充分覆盖的广告与营销接口做补充。目标是在 MITM 范围尽量小、不影响登录 / 支付 / 订票 / 导航等核心功能的前提下，去掉开屏、弹窗、广告卡片和营销推荐。

## 推荐组合

三个覆写配合使用，缺一不可，也不要再叠加其他大型通用去广告覆写：

| 层 | 覆写 | 一键安装 |
| --- | --- | --- |
| 基础 | Shiina AdBlock Lite | [安装](https://link.stash.ws/install-override/raw.githubusercontent.com/ShiinaWong/stash-configs/main/overrides/shiina-adblock-lite.stoverride) |
| 开屏补充 | Shiina Startup Ads | [安装](https://link.stash.ws/install-override/raw.githubusercontent.com/ShiinaWong/stash-configs/main/overrides/modules/startup-ads.stoverride) |
| 个人补充 | Extra AdBlock（本仓库） | [安装](https://link.stash.ws/install-override/raw.githubusercontent.com/wiwiwiwiwii/Ad-blocker/main/Extra-AdBlock.stoverride) |

Extra AdBlock Raw 地址：

```text
https://raw.githubusercontent.com/wiwiwiwiwii/Ad-blocker/main/Extra-AdBlock.stoverride
```

使用前需在 Stash 中启用 MITM 并信任 CA 证书。覆写内容更新后，在 Stash 里刷新已安装的覆写即可，不要重复安装，也无需重装证书。

## 不建议同时启用

以下覆写与上述组合重复，同时启用会重复处理请求、扩大 MITM 范围：

- Blackmatrix7 `AdvertisingLite` / `AllInOne` / 旧版「开屏去广告」
- 已启用 Shiina Lite 时的独立 Shiina 淘宝、京东模块
- 独立的中国移动去广告模块（Extra 已包含）

## Extra AdBlock 覆盖范围

| 类别 | App |
| --- | --- |
| 财经 / 资讯 | 金十数据、东方财富、同花顺、雪球、财联社、华尔街见闻、界面新闻、天天基金 |
| 出行 / 生活 | 航旅纵横、铁路 12306、交管 12123、美团、大众点评、高德地图、滴滴、南方航空、东方航空、华住会、希尔顿、亚朵 |
| 运营商 | 中国移动、中国联通 |
| 社交 / 漫画 | 微博、哔哩哔哩漫画 |
| 音乐 / 阅读 / 工具 | 喜马拉雅、网易云音乐、QQ 音乐、掌阅、网易邮箱大师、百度网盘、米家、Fitdays |
| 求职 / 购物 | 猎聘、山姆会员商店 |
| Shiina Lite 已覆盖 App 的补充 | 京东、拼多多、闲鱼、小红书（仅搜索营销、推荐营销等 Shiina Lite 未处理的接口） |

Extra 不做 VIP / 会员伪造、付费功能解锁、支付或订单修改、登录绕过、关闭证书校验等操作；银行、支付、券商、政务、医疗类 App 默认不做 MITM。

## 出现问题时

如果某个 App 功能异常，先单独关闭 Extra AdBlock 复现：问题消失则属于本仓库规则，保留 Shiina 两个模块不动即可排查。

## 维护

规则准入标准、上游来源、部署前检查清单见 [AGENTS.md](AGENTS.md)。
