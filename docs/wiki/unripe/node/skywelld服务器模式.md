# skywelld服务器模式

`skywelld`服务器软件可以根据其配置以多种模式运行，包括：

* 公共节点服务器 - 同步网络分类帐到本地。
* 验证服务器或简称验证器 - 参与共识。
* `skywelld`服务器处于独立模式 - 用于测试。不与其他`skywelld`服务器通信。

您还可以将`skywelld`可执行文件作为客户端应用程序运行，以便在本地访问[`skywelld` API]。 （在这种情况下，同一个二进制文件的两个实例可以并行运行;一个作为服务器运行，另一个作为客户端短暂运行然后终止。）


## 运行公共节点skywelld服务器的原因

你可能想要运行自己的`skywelld`服务器有很多原因，但大多数可以归结为：你可以信任自己的服务器，你可以控制它的工作量，你不受别人的支配决定何时以及如何访问它。当然，您必须养成良好的网络安全性，以保护您的服务器免受恶意黑客攻击。

你需要相信你使用的“skywelld”。如果您连接到恶意服务器，有很多方法可以利用您并且导致您赔钱。例如：

* 恶意服务器可能会在没有付款时报告您已付款。
* 它可以有选择地显示或隐藏支付路径和货币兑换优惠，以保证自己的利润，同时不为您提供最优惠的价格。
* 如果您泄露了地址的密钥，它可能代表您进行任意交易，甚至转移或销毁您的地址所持有的所有资金。

此外，运行您自己的服务器可让您对其进行管理控制，从而允许您运行重要的仅限管理员和负载密集型命令。如果您使用共享服务器，则必须担心同一服务器的其他用户与您竞争服务器的计算能力。 WebSocket API中的许多命令会给服务器带来很大压力，因此`skywelld`可以选择在需要时缩减其响应。如果您与他人共享服务器，则可能无法始终获得最佳结果。

最后，如果运行验证服务器，则可以使用公共节点服务器作为公共网络的代理，同时将验证服务器保留在私有子网上，只能通过公共节点服务器访问外部网络。这使得更难以破坏验证服务器的完整性。

## 运行验证器的原因

SWTC Ledger的健壮性取决于一个互连的验证器网络，每个验证器都信任一些其他验证器不要串通。谁运行验证器拥有不同地域的运营商越多，网络的每个成员就越可以确定它继续公正地运行。如果您或您的组织依赖于SWTC Ledger，那么为您的共识流程做出贡献符合您的利益。

并非所有“skywelld”服务器都需要验证器：信任来自同一运营商的许多服务器并不能提供更好的防止串通的保护。在发生自然灾害和其他紧急情况时，组织可以在多个地区运行验证器以实现冗余。

如果您的组织运行验证服务器，您还可以运行一个或多个公共节点服务器，以平衡API访问的计算负载，或者作为验证服务器与外部网络之间的代理。


### 一个好的验证器的属性

有几个属性定义了一个好的验证器。您的服务器所包含的这些属性越多，其他人必须将您的服务器包含在可信验证器列表中的原因就越多：

* **可用性**：应始终运行理想的验证器，为每个提议的分类帐提交验证投票。
    * 力争100％正常运行时间。
* **协议**：验证者的投票应尽可能与共识过程的结果相匹配。否则可能表明验证者的软件已过时，错误或故意偏向。
    * 始终运行最新的`skywelld`版本而不做任何修改。
* **及时性**：验证者的投票应该很快到达，而不是在达成共识轮次之后。
    * 快速的互联网连接有助于此。
* **确定**：应该清楚谁运行验证器。理想情况下，可信验证者列表应包括由多个法律管辖区域和地理区域中的不同所有者运营的验证者，以减少任何本地化事件可能干扰验证者公正操作的可能性。
    * 设置[域名验证]是一个好的开始。

目前，不能推荐除默认验证器列表中的验证器之外的任何验证器。但是，我们正在收集其他验证器和构建工具的数据，以报告其性能。


## 以独立模式运行`skywelld`服务器的原因

您可以在独立模式下运行`skywelld`，而无需受信任服务器的共识。在独立模式下，`skywelld`不与SWTC Ledger对等网络中的任何其他服务器通信，但您只能在本地服务器上执行大多数相同的操作。独立提供了一种测试“skywelld”行为的方法，而不必依赖于实时网络。例如，在这些修正案在分散网络中生效之前，您可以[测试修正案的效果]。

当您在独立模式下运行`skywelld`时，您必须告诉它从哪个分类帐版本开始：

* 从头开始创建[new genesis ledger]。
* [从硬盘加载现有分类帐版本]。

**注意**：在独立模式下，您必须[手动推进分类帐]（advance-the-ledger-in-stand-alone-mode.html）。

## 也可以看看

- [命令行用法参考] - 有关所有skywelld服务器模式的命令行选项的详细信息。

