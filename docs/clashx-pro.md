# 接入 ClashX Pro

ClashX Pro 使用订阅地址拉取配置，不登录面板。服务器按 [部署](deploy.md) 启动后，先确认内核，再去 [创建账户](account.md)，最后回到下面的「导入订阅」。

## 确认内核

点菜单栏的 ClashX Pro 图标，打开「关于」或启动日志。

| 关于里显示的内核 | 接下来在面板里建 |
| --- | --- |
| Clash Premium | [Trojan 或 Shadowsocks](account.md#premium-已有域名) |
| Meta 或 Mihomo | [VLESS + REALITY](account.md#meta--mihomo) |

经典 ClashX Pro 是 Premium。较新的版本可以在菜单里切换到 Meta，切换后重新导入订阅。

Premium 的订阅路径是 `/clash-legacy/<订阅 ID>`，可用协议是 Shadowsocks、VMess、Trojan。Meta / Mihomo 的路径是 `/mihomo/<订阅 ID>`，还可以使用 VLESS、REALITY、Hysteria2。

## 导入订阅

1. 菜单栏图标 → 配置（Config）→ 远程配置（Remote Config）→ 添加。
2. 粘贴 [创建账户](account.md#4-复制订阅地址) 里复制的地址，保存。
3. 选中这份远程配置并更新。
4. 打开「设置为系统代理」。需要接管全部流量时再开增强模式（TUN），并允许系统要求的权限。
5. 在代理组里选中新节点，做一次延迟测试。

更新订阅不会切换内核。换内核后使用对应的订阅地址重新导入。

要在订阅里下发分流规则时，打开「面板设置 → 订阅 → Clash / Mihomo」，启用路由并填入规则。保存后在 ClashX Pro 里更新订阅。

导入失败时对照 [排障](deploy.md#排障)。
