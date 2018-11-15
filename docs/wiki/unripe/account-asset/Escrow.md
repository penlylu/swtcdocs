# Escrow

在Escrow对象类型代表手持支付的XRP等待被执行或取消。一个EscrowCreate交易创建一个Escrow总帐对象。成功的EscrowFinish或EscrowCancel事务将删除该对象。如果Escrow对象具有[_加密条件_](https://tools.ietf.org/html/draft-thomas-crypto-conditions-02)，则只有在EscrowFinish事务提供满足条件的相应履行时，才能成功付款。（唯一受支持的加密条件类型是[PREIMAGE-SHA-256](https://tools.ietf.org/html/draft-thomas-crypto-conditions-02#section-8.1)。）如果Escrow对象有FinishAfter时间，则保留的付款只能在该时间之后执行。

一个`Escrow`对象与两个地址相关联：

- 所有者，在创建Escrow对象时提供SWTC 。如果取消保留的付款，则SWTC将返回给所有者。
- 目的地，当持有的付款成功时支付SWTC。目的地可以与所有者相同。


## 示例 Escrow JSON

```json
{
    "Account": "jGyBtKKtrabR5VHbvKJ6wizUUBnrCWRTdw",
    "Amount": "10000",
    "CancelAfter": 545440232,
    "Condition": "A0258020A82A88B2DF843A54F58772E4A3861866ECDB4157645DD9AE528C1D3AEEDABAB6810120",
    "Destination": "jLqM1U6Kp3BWCEsAvz8W1o7DhsQSA9GuPM",
    "DestinationTag": 23480,
    "FinishAfter": 545354132,
    "Flags": 0,
    "LedgerEntryType": "Escrow",
    "OwnerNode": "0000000000000000",
    "DestinationNode": "0000000000000000",
    "PreviousTxnID": "C44F2EB84196B9AD820313DBEBA6316A15C9A2D35787579ED172B87A30131DA7",
    "PreviousTxnLgrSeq": 28991004,
    "SourceTag": 11747,
    "LedgerIndex": "DC5F3851D8A1AB622F957761E5963BC5BD439D5C24AC6AD7AC4523F0640244AC"
}
```

## Escrow 字段

一个`Escrow`对象具有以下字段：

| 名称              | JSON 类型 | 类型 | 描述 |
|-------------------|-----------|---------------|-------------|
| `LedgerEntryType`   | String    | UInt16    | 账本对象类型。 |
| `Account`           | String | AccountID | 账户地址。|
| `Destination`       | String | AccountID | 发送交易的目标地址 |
| `Amount`            | String | Amount    | 发送交易的SWTC数量。 |
| `Condition`         | String | VariableLength | _(可选)_ [PREIMAGE-SHA-256 crypto-condition](https://tools.ietf.org/html/draft-thomas-crypto-conditions-02#section-8.1)，为十六进制。如果存在，EscrowFinish事务必须包含满足的条件。 |
| `CancelAfter`       | Number | UInt32 | _(可选)_ 当且仅当此字段指定的时间已过时，才能取消持有的付款。 |
| `FinishAfter`       | Number | UInt32 | _(可选)_ 这个数值一般设置在支付完成之后。此时间之前的任何EscrowFinish交易都会失败。 |
| `Flags`             | Number | UInt32 | 标志。没有为托管类型定义的标志，因此该值始终为`0`。 |
| `SourceTag`         | Number | UInt32 | _(可选)_ 一个任意标记，用于进一步指定此持有付款的来源，例如所有者地址的托管收件人。 |
| `DestinationTag`    | Number | UInt32 | _(可选)_ 一个任意标记，用于进一步指定此持有付款的目标，例如目标地址的托管收件人。 |
| `OwnerNode`         | String    | UInt64    | 如果目录由多个页面组成，则提示所有者目录的哪个页面链接到此对象。 |
| `DestinationNode`   | String    | UInt64    | _(可选)_ 如果目录由多个页面组成，则提示目标所有者目录的哪个页面链接到此对象。 |
| `PreviousTxnID`     | String | Hash256 | 上一笔修改对象交易的哈希值。 |
| `PreviousTxnLgrSeq` | Number | UInt32 | 上一笔修改对象交易所对应的账本索引。 |

