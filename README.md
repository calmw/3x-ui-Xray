# 3x-ui-Xray

给 Clash 客户端用的 Xray 服务端。面板来自上游 [3X-UI](https://github.com/MHSanaei/3x-ui)，代理内核是 [Xray-core](https://github.com/XTLS/Xray-core)。在 Linux 服务器上用 Docker Compose 部署。

推荐客户端是 [Clash Verge Rev](https://github.com/clash-verge-rev/clash-verge-rev/releases)。它使用 Mihomo 内核，可以连接 VLESS + REALITY。ClashX Pro 使用 Premium 内核，只能连接 Trojan、VMess 和 Shadowsocks。

上游文档：<https://docs.sanaei.dev>

## 按这个顺序做

1. [部署](docs/deploy.md)：在服务器上启动面板。
2. [创建账户](docs/account.md)：建 VLESS + REALITY 入站，添加客户端，复制订阅地址。
3. [导入订阅](docs/clients.md#导入到-clash-verge-rev)：在 Clash Verge Rev 里使用 `/mihomo/<订阅 ID>`。

面板是 `http://<服务器IP>:2053/panel/`。容器启动后先重置用户名和密码，再登录。订阅端口是 **2096**。数据库在 `db/`，证书在 `cert/`。

| 客户端 | 入站 | 订阅路径 |
| --- | --- | --- |
| Clash Verge Rev | VLESS + REALITY | `/mihomo/<订阅 ID>` |
| ClashX Pro | Trojan + TLS，或 Shadowsocks | `/clash-legacy/<订阅 ID>` |

`/sub/<订阅 ID>` 返回分享链接。客户端使用上表里的路径。各客户端的导入步骤见 [客户端](docs/clients.md)。
