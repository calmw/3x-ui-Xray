# 3x-ui-Xray

给 ClashX Pro 用的 Xray 服务端。面板来自上游 [3X-UI](https://github.com/MHSanaei/3x-ui)，代理内核是 [Xray-core](https://github.com/XTLS/Xray-core)。在 Linux 服务器上用 Docker Compose 部署，Mac 只运行 ClashX Pro。

上游文档：<https://docs.sanaei.dev>

## 按这个顺序做

1. [部署](docs/deploy.md)：在服务器上启动面板。
2. [确认内核](docs/clashx-pro.md#确认内核)：在 Mac 的 ClashX Pro「关于」里看内核，后面的入站按这个选。
3. [创建账户](docs/account.md)：建入站、添加客户端、复制订阅地址。
4. [导入订阅](docs/clashx-pro.md#导入订阅)：把地址填进 ClashX Pro。

面板是 `http://<服务器IP>:2053/panel/`，首次账号 `admin` / `admin`。订阅端口是 **2096**。数据库在 `db/`，证书在 `cert/`。

| 内核 | 入站 | 订阅路径 |
| --- | --- | --- |
| Clash Premium | Trojan + TLS，或 Shadowsocks | `/clash-legacy/<订阅 ID>` |
| Meta / Mihomo | VLESS + REALITY | `/mihomo/<订阅 ID>` |

`/sub/<订阅 ID>` 返回分享链接。ClashX Pro 使用上表里的路径。
