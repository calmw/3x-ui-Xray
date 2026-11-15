# 客户端

服务器按 [部署](deploy.md) 启动，账户按 [创建账户](account.md) 建好后，用订阅地址导入。客户端不登录面板。

默认使用 **Clash Verge Rev**，配合 VLESS + REALITY。ClashX Pro 只能使用 Shadowsocks、VMess、Trojan。

两套软件不要同时打开系统代理。导入前先退出另一套。

## 支持情况

| 客户端 | 内核 | 入站 | 订阅路径 |
| --- | --- | --- | --- |
| [Clash Verge Rev](https://github.com/clash-verge-rev/clash-verge-rev/releases) | Mihomo | VLESS + REALITY | `/mihomo/<订阅 ID>`，面板详情里的 **CLASH** 行也是这一份 |
| ClashX Pro | Premium | Trojan + TLS，或 Shadowsocks、VMess | `/clash-legacy/<订阅 ID>` |

`/sub/<订阅 ID>` 是分享链接，这两个客户端都不用它。

Clash Verge Rev 从上面的发布页下载 macOS 安装包。Apple 芯片选文件名里带 `aarch64` 的 dmg，Intel 芯片选 `x64`。

## 导入到 Clash Verge Rev

1. 打开 Clash Verge Rev，进入 **订阅**。
2. 新建一条远程订阅，把 [创建账户](account.md#4-复制订阅地址) 里的 `/mihomo/<订阅 ID>` 粘贴进去。也可以用客户端详情里 **CLASH** 行复制的地址。
3. 保存后更新这条订阅，并选中它。
4. 打开 **系统代理**。需要接管全部流量时再开 TUN。
5. 在代理组里选中节点，做一次延迟测试。

面板以后改了节点，在 Clash Verge Rev 里更新这条订阅即可。

## 导入到 ClashX Pro

ClashX Pro 的菜单栏图标里没有「远程配置」。路径是 **配置 → 托管配置 → 管理 → 添加**。英文界面是 **Config → Managed Config → Manage → Add**。

URL 使用 `/clash-legacy/<订阅 ID>`。这个客户端导入 VLESS 时会提示 `unsupport proxy type: vless`。入站需要是 Trojan、VMess 或 Shadowsocks，建法见 [创建账户](account.md#clashx-pro-已有域名)。

添加完成后，回到 **配置** 菜单选中这份托管配置，再勾选 **设置为系统代理**。需要接管全部流量时再开 **增强模式**。

已经下载了 YAML 文件时，打开 **配置 → 打开配置文件夹**，把文件放进去，再在 **配置** 菜单里点选它。本地文件不会随面板更新。

## 分流规则

要在订阅里下发分流时，打开「面板设置 → 订阅 → Clash / Mihomo」，启用路由并填入规则。保存后在客户端里更新订阅。
