# 部署

在有公网 IP 的 Linux 服务器上，用 [deploy/docker-compose.yml](../deploy/docker-compose.yml) 启动 3X-UI。镜像里带 Xray-core。编排使用 host 网络，代理流量直接走宿主机网卡。

下一步是 [创建账户](account.md)，然后用 [Clash Verge Rev](clients.md#导入到-clash-verge-rev) 导入订阅。

## 环境

- 系统：Ubuntu、Debian，或其它能安装 Docker Engine 的 Linux。常见架构是 `amd64`、`arm64`。
- 权限：root，或可以管理 Docker 的用户。
- 开放这些端口。系统防火墙和云安全组都要放行。

| 端口 | 用途 |
| --- | --- |
| 2053/tcp | 面板，路径 `/panel/` |
| 2096/tcp | 订阅。客户端从这里拉配置 |
| 入站端口 | 代理流量，在面板里创建入站时指定，常用 443 |

面板和订阅默认走 HTTP。有域名后为两者配置证书，账号和节点参数走加密连接。

## 安装 Docker

服务器上还没有 Docker Engine 和 Compose 插件时：

```bash
bash <(curl -sSL https://get.docker.com)
docker compose version
```

## 放行端口

下面以 443 作为入站端口。实际端口不同时，换成面板里填写的数字。

Ubuntu / Debian（ufw）：

```bash
ufw allow 2053/tcp
ufw allow 2096/tcp
ufw allow 443/tcp
```

firewalld：

```bash
firewall-cmd --permanent --add-port=2053/tcp
firewall-cmd --permanent --add-port=2096/tcp
firewall-cmd --permanent --add-port=443/tcp
firewall-cmd --reload
```

2053 可以只对管理员的 IP 开放。2096 和入站端口需要对客户端所在的网络开放。

## 启动

把本仓库放到服务器上，在仓库根目录执行：

```bash
docker compose -f deploy/docker-compose.yml up -d
```

数据写在仓库下的 `db/`，证书放 `cert/`。这两个目录已在 `.gitignore` 中。新建入站后端口直接出现在宿主机上，编排文件保持不变。

容器起来后，先把面板账号重置成自己的，再登录。下面两条在运行容器的机器上执行，把引号里的密码换成自己的：

```bash
docker exec 3x-ui /app/x-ui setting -username admin -password '你的新密码'
docker restart 3x-ui
```

`restart` 完成后再打开：

```text
http://<服务器IP>:2053/panel/
```

用户名填 `admin`，密码填上一步引号里设置的值。登录后可以在「面板设置」里打开二次验证。`/panel/` 只在数据库第一次生成时写入。数据库已经存在时，在面板设置里修改访问路径。

## 日常维护

```bash
docker logs -f 3x-ui
docker compose -f deploy/docker-compose.yml pull
docker compose -f deploy/docker-compose.yml up -d
docker compose -f deploy/docker-compose.yml down
```

`pull` 之后再 `up -d` 会换成新镜像，`db/` 仍留在宿主机。`down` 同样保留 `db/`。清空面板时，先 `down`，再删除 `db/`。

备份 `db/` 和 `cert/`。恢复时先 `down`，把目录放回仓库下，再 `up -d`。

## 排障

| 现象 | 处理 |
| --- | --- |
| 浏览器打不开面板 | 放行 2053。`docker ps` 中能看到 `3x-ui`。地址带上 `/panel/`。 |
| Clash 更新订阅失败 | 放行 2096。在「面板设置 → 订阅」中打开订阅服务和 Clash / Mihomo。地址使用 `/clash-legacy/` 或 `/mihomo/`。 |
| 订阅能更新，节点超时 | 放行入站端口。入站和客户端都是启用状态，未过期，流量未超限。服务器上执行 `ss -lnt`，能看到该端口。 |
| ClashX Pro 提示 `unsupport proxy type: vless` | 改用 [Clash Verge Rev](clients.md#导入到-clash-verge-rev)，订阅地址用 `/mihomo/<订阅 ID>`。继续用 ClashX Pro 时，入站改为 Trojan 或 Shadowsocks，地址用 `/clash-legacy/<订阅 ID>`。 |
| Clash 提示 YAML 无效 | Clash Verge Rev 使用 `/mihomo/<订阅 ID>`。ClashX Pro 使用 `/clash-legacy/<订阅 ID>`。 |
| `/clash-legacy/` 返回 422 | 该客户端需要挂一个 Trojan、VMess 或 Shadowsocks 入站。 |
| 打开订阅只有说明页 | 当前地址是 `/sub/<订阅 ID>`。换成上表中的 Clash 路径。 |
