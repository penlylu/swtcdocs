# Peer协议

SWTC Ledger中的服务器使用SWTC Ledger对等协议（也称为RTXP）相互通信。对等服务器通过HTTPS连接，然后执行[HTTP升级]切换到RTXP。 （有关更多信息，请参阅[`skywelld`存储库]中的[Overlay Network]

## 配置对等协议

要参与SWTC Ledger，skywelld服务器使用对等协议连接到任意对等方。（所有这些对等体都被视为不受信任，除非它们与当前服务器集群在一起。）

理想情况下，服务器应该能够在对等端口上发送和接收连接。您应该将用于对等协议的端口通过防火墙转发到skywelld服务器。该缺省skywelld配置文件侦听所有网络接口上的端口50333传入等协议的连接。您可以通过编辑skywelld.cfg文件中的相应节来更改使用的端口。

例：

```
[port_peer]
port = 50333
ip = 0.0.0.0
protocol = peer
```

## Peer Crawler

Peer Crawler要求skywelld服务器将skywelld与其连接的其他服务器的信息报告为对等方。该peers命令中的WebSocket和JSON-RPC的API也返回了类似的，更全面的信息，但需要管理访问到服务器。Peer Crawler响应可通过对等协议（RTXP）端口以非特权方式提供给其他服务器。

#### 请求格式

要请求Peer Crawler信息，请发出以下HTTP请求：

* **协议**： https
* **HTTP方法**： GET
* **主机**：（任何`skywelld`服务器，按主机名或IP地址）
* **端口**：（`skywelld`服务器使用对等协议的端口号，通常为50333）
* **路径**：`/ crawl`
* **注意**：大多数`skywelld`服务器使用自签名证书来响应请求。默认情况下，大多数工具（包括Web浏览器）会标记或阻止此类响应为不受信任。您必须忽略证书检查（例如，如果使用cURL，请添加`--insecure`标志）以显示来自这些服务器的响应。

#### 响应格式


The response has the status code **200 OK** and a JSON object in the message body.

The JSON object has the following fields:

| `Field`          | Value | Description                                       |
|:-----------------|:------|:--------------------------------------------------|
| `overlay.active` | Array | 对等对象数组，其中每个成员是连接到此服务器的对等方。 |

Each member of the `active` array is a Peer Object with the following fields:

| `Field`      | Value                    | Description                        |
|:-------------|:-------------------------|:-----------------------------------|
| `ip`         | String (IPv4 Address)    | 此连接对等体的IP地址。 |
| `port`       | String (Number)          | 服务于RTXP的对等服务器上的端口号。通常为50333。 |
| `public_key` | String (Base-64 Encoded) | 此对等方用于签署RTXP消息的ECDSA密钥对的公钥。（这pubkey_node与对等服务器server_info命令中报告的数据相同。） |
| `type`       | String                   | 值in或out，指示与对等方的TCP连接是传入还是传出。 |
| `uptime`     | Number                   | 服务器已连接到此对等方的秒数。|
| `version`    | String                   | skywelld对等方报告使用的版本号。

## 私人对等节点

您可以使用配置文件的[peer_private]节skywelld来请求对等服务器不在Peer Crawler响应中报告您的IP地址。可以修改您无法控制的服务器，使其不遵守此设置。但是，您可以使用此选项隐藏skywelld您控制的服务器的IP地址，这些服务器仅连接到您控制的对等端（使用[ips_fixed]和防火墙）。通过这种方式，您可以使用您控制的公共skywelld服务器作为私有skywelld服务器的代理。

为了保护重要的验证服务器，建议配置一个或多个公共节点服务器作为代理，而验证服务器受防火墙保护，只连接到公共节点服务器。

配置示例：

```
＃仅在连接的私有服务器上配置
#IP地址为10.1.10.55的第二个skywelld服务器
[ips_fixed]
10.1.10.55

[peer_private]
1
```
