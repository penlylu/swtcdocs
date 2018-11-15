# 账户

***

SWTC账本中的“账户”代表SWTC的持有者和[交易]()的发送者。账户的核心要素是：

- 唯一的地址，例如j1gh2NuxKrCxDv67kzs5wsnhqJPGDJarU
- SWTC的余额，每个账户至少要[保留一定的swtc](Reserves.md)。
- 序列号，从1开始，随着从该账户发送的每个交易增加而增加。除非交易的序列号与其发送方的下一个序列号相匹配，否则交易不能包含在账本中。
- 交易的历史，可以影响本账户的余额。
- 一种或多种授权交易的方式，可能包括：
	- 账户固有的主密钥对。（可以禁用此功能，但不能更改。）
	- 可以旋转的 “常规”密钥对。
	- 用于[多重签名](Multi-Signing.md)的签名者列表。（与账户的核心数据分开存储。）

在账本所储存的数据中，账户的核心数据存储在[AccountRoot](AccountRoot.md)账本对象类型中。账户也可以被其他几种类型数据所拥有或部分拥有。

**提示:** 非SWTC货币的资产不存储在SWTC账本中; 每种此类资产都存储在称为“[Trust Line](资产.md)”的账本中，该关系连接着SWTC账本。
	
## 创建账户

没有专门的“创建账户”交易。如果给一个有效的SWTC地址发送大于等于20个SWTC，那么系统会为其自动创建一个账户。这种账户称为资金账户，并在账本中创建[AccountRoot](AccountRoot.md)对象。

**警告:** 资金并不会为您提供该账户的任何特权。拥有该账户地址所对应私钥的人可以完全控制账户及其所包含的SWTC。对于某些地址，可能没有人拥有密钥，在这种情况下，该账户是黑洞，SWTC永远丢失。

在SWTC账本中获取账户的典型方法如下：

1. 从强大的随机源中生成密钥对，并计算该密钥对的地址。

2. 让那些拥有SWTC资金账户的人将SWTC发送到您生成的地址中。

    - 例如，您可以在私人交易所购买SWTC，然后将SWTC从交易所提币到您指定的地址。

        **警告:** 第一次在您自己的SWTC账本地址上收到SWTC时，您必须预留一定的账户储备金（当前为20SWTC），这20SWTC将被无限期地锁定。相比之下，私人交易所通常将所有客户的SWTC保存在一些共享的SWTC账户中，因此客户无需为交易所的个人账户预留准备金。在提币之前，请考虑直接在SWTC账本上拥有自己的账户是否物有所值。


## 地址

SWTC账本中的账户由base58定义。该地址来自账户的公钥，该公钥又来自私钥。地址在JSON中表示为字符串，具有以下特征：

- 长度在25到35个字符之间
- 从角色开始j
- 使用字母数字字符，不包括数字“0”大写字母“O”，大写字母“I”和小写字母“l”
- 区分大小写
- 包括一个4字节的校验和，以便从随机字符生成有效地址的概率约为2^32

任何有效地址都可以通过获得资助成为SWTC账本中的账户。您还可以使用未经资助的地址来表示常规密钥或[签名者列表](Multi-Signing.md)的成员。只有资金账户才能发送交易。

从密钥对开始，创建有效地址是一个不可逆的数学过程。您可以完全脱机生成密钥对并计算其地址，而无需与SWTC账本或任何其他方通信。从公钥到地址的转换涉及单向散列函数，因此可以确认公钥与地址匹配，但是不可能仅从地址导出公钥。（这是签名交易包括公钥和发送交易方的部分原因。）

有关如何计算SWTC地址的更多技术细节，请参阅地址编码。


## 特殊地址

某些地址在SWTC账本中具有特殊含义或历史用途。在许多情况下，这些是“黑洞”地址，意味着地址不是从已知的密钥导出的。由于实际上不可能仅从地址猜测密钥，因此黑洞地址所拥有的任何SWTC都将永远丢失。

| 地址                     | 名称 | 含义 | 是否黑洞 |
|-----------------------------|------|---------|-------------|
| jjjjjjjjjjjjjjjjjjjjjhoLvTp | ACCOUNT\_ZERO | 这个地址base58编码值为0。在点对点网络通信中，jingtum使用此地址作为SWTC的颁发者。 | 是 |
| jjjjjjjjjjjjjjjjjjjjBZbvri  | ACCOUNT\_ONE | 这个地址base58编码值为1。在账本中，SkywellState使用此地址用来作为TrustLine发行者的占位符。 | 是 |
| jHb9CJAWyB4jr91VRWn96DkukG4bwdtyTh | 创世地址 | 当jingtum从头开始创建新的账本时（例如，在独立模式下），此账户将保留所有SWTC。该地址是从硬编码的种子值“masterpassphrase”生成的。| 否 |
| jjjjjjjjjjjjjjjjjjjn5RM1jHd | NaN Address | 当base58编码值NaN时，先前版本的[jingtum-lib](https://github.com/swtcpro/jingtum-api)会生成此地址。 | 是 |


## 账户的持久性

账户一旦创建后，会永远存在于SWTC账本中。以便根据序列号永久追踪交易，避免双花问题。

与比特币和许多其他加密货币不同，SWTC公共账本链的每个新版本都包含账本的完整状态，每个新账户的大小都会增加。因此，除非完全有必要，否则jingtum不鼓励创建新账户。代表许多用户发送和接收价值的机构可以使用源标签和目标标签来区分与客户的付款，同时仅使用SWTC账本中的一个（或少数）账户。



## 交易历史

在SWTC账本中，交易历史记录通过交易的唯一哈希值和账本索引链接的区块链追踪。该[AccountRoot](AccountRoot.md)账本对象能够识别哈希和最近修改它的交易账本; 该交易的元数据包含AccountRoot节点先前的状态，因此可以通过这种方式遍历单个账户的历史记录。此交易历史记录包括AccountRoot直接修改节点的所有交易，包括：

- 账户发送的交易，因为他们修改了账户的序列号。由于[交易费]()，这些交易还会修改账户的SWTC余额。
- 修改账户SWTC余额的交易，包括收款付款交易和其他类型的交易，如[PaymentChannelClaim](PaymentChannelClaim.md)和[EscrowFinish](EscrowFinish.md)。

账户的交易历史记录还包括修改账户拥有的对象和非转账交易。这些对象是单独账本对象，每个对象关联着它们的之前交易。如果您拥有账户的完整账本，则可以向前追踪，可以查找到之前被修改过的账户对象。“完整”的交易历史记录包括历史对象交易，例如：

- `SkywellState` 连接到账户的对象（Trust Lines）。
- `DirectoryNode` 地址的目录，保存地址对象的信息。
- `Offer` 代表去中心化交易所中的挂单订单
- `PayChannel` 表示账户之间的异步支付渠道
- `Escrow` 表示账户发起的付款被锁定一段时间或者需要某种条件触发。
- `SignerList` 表示可以通过多重签名[]授权账户交易的地址列表。



## 地址编码

**提示:** 这些技术细节仅适用于为SWTC账本兼容性构建低级库软件的人员！


SWTC账户地址使用base58和jingtum字典进行编码：jpshnaf39wBUDNEGHJKLM4PQRST7VWXYZ2bcdeCg65rkm8oFqi1tuvAxyz。由于SWTC账本使用base58对几种类型的密钥进行编码，因此它使用一个字节的“类型前缀”（也称为“版本前缀”）为编码数据添加前缀以区分它们。类型前缀导致地址通常以base58格式的不同字母开头。

下图显示了密钥和地址之间的关系：

![Passphrase → Secret Key → Public Key + Type Prefix → Account ID + Checksum → Address](swtc-address.png)

计算SWTC账户地址的公式如下。有关完整的示例代码，请参阅[`secp256KeyPair.go`](https://github.com/swtcpro/jingtum-lib-go/blob/master/src/jingtumLib/crypto/secp256k1/secp256KeyPair.go)。

	    pubBytes := pub.ToBytes()
	
		/* SHA256 Hash */
		sha256H := sha256.New()
		sha256H.Reset()
		sha256H.Write(pubBytes)
		pubHash1 := sha256H.Sum(nil)
	
		/* RIPEMD-160 Hash */
		ripemd160H := ripemd160.New()
		ripemd160H.Reset()
		ripemd160H.Write(pubHash1)
		pubHash2 := ripemd160H.Sum(nil)
		address = jtUtils.EncodeB58(jtConst.AccountPrefix, pubHash2)
