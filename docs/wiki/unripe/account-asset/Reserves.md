# Reserves

SWTC账本在SWTC中应用保留要求，用以保护共享的全局账本免于因垃圾邮件或恶意攻击而急剧增大。目标是限制账本的增长以匹配技术的改进，以便当前的商用计算机始在磁盘中能存下整个账本。

一个账户必须在全局共享账本中保留最小量的SWTC才能发送交易，否则您将无法将此SWTC发送到其他地址。要为新地址提供资金，您必须发送足够的SWTC以满足保留要求。

目前的最低储备要求是 20 SWTC。（这是在账本中不拥有其他对象地址的成本。）


## Base Reserve and Owner Reserve

reserve要求分为两部分：

* **Base Reserve** 每个地址SWTC的最小保留量。目前是 20 SWTC(`20000000` drops)。
* **Owner Reserve** 存款准备金，用于增加对象。目前，每个对象 5 SWTC(`5000000` drops)。


### Owner Reserves

账本中的许多对象由特定地址拥有，并计入该地址的保留要求。从账本中删除对象时，它们不再计入其所有者的保留要求。

- [Offers](Offer.md) 由放置它们的地址所有。 如果挂单被交易掉或取消掉，挂单交易会自动被删除。或者，所有者可以通过发送OfferCancel事务或通过发送包含参数OfferSequence的OfferCreate事务来取消挂单交易OfferSequence。
- [Trust lines](资产.md) 在两个地址之间共享。所有者保留可以应用于一个或两个地址，具体取决于地址控件的字段是否处于默认状态。
- 签名者列表，单个[SignerList](SignerList.md)有3到10个对象不等。
- [Held Payments (Escrow)](Escrow.md) 由放置它们的地址所有。
- [Payment Channels](PaymentChannelClaim.md) 创建它们的地址所有。
- [Owner directories](DirectoryNode.md) 列出所有对象。但是，所有者目录本身不计入其中。
- [Checks](Checks.md) 由创建它们的地址拥有。


## 低于Reserve要求

在事务处理期间，交易成本会消耗一点发送地址的SWTC余额。这可能导致地址的SWTC低于保留要求。

当地址保留的SWTC少于当前保留要求时，它将不能发送交易。即便如此，地址仍然存在于账本中，只要有足够的SWTC来支付交易成本，就可以发送交易。如果地址收到足够的SWTC以再次满足其储备要求，或者如果保留要求减少到低于地址的SWTC持有量，则该地址可以再次发送所有类型的交易。


## 改变Reserve要求

SWTC账本有一种机制可以调整SWTC值适应长期变化的储备要求。任何变更都必须得到共识流程的批准。

