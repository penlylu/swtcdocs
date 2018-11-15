# EscrowFinish

将SWTC从持有人付款给接收者。


## 示例EscrowFinish JSON

```json
{
    "Account": "j3N35VHut94dD1Y9H1KoWmGZE2kNNRFcVk",
    "TransactionType": "EscrowFinish",
    "Owner": "j3N35VHut94dD1Y9H1KoWmGZE2kNNRFcVk",
    "OfferSequence": 7,
    "Condition": "A0258020E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855810100",
    "Fulfillment": "A0028000"
}
```

## EscrowFinish字段
除了公共字段外，EscrowFinish交易还使用以下字段：


| 字段           | JSON类型        | 类型 | 描述               |
|:----------------|:-----------------|:------------------|:--------------------------|
| `Owner`         | String           | AccountID         | 源帐户的地址。
| `OfferSequence` | Unsigned Integer | UInt32            | 创建EscrowFinish交易的序列号。
| `Condition`     | String           | VariableLength    | _(可选)_ 十六进制值与先前提供的[PREIMAGE-SHA-256](https://tools.ietf.org/html/draft-thomas-crypto-conditions-02#section-8.1)加密条件匹配。|
| `Fulfillment`   | String           | VariableLength    | _(可选)_ [PREIMAGE-SHA-256](https://tools.ietf.org/html/draft-thomas-crypto-conditions-02#section-8.1)加密条件满足的十六进制值与Condition匹配。 |

任何帐户都可以提交EscrowFinish交易。

- 如果持有的付款有FinishAfter时间，则您无法在此之前执行。具体来说，如果相应的EscrowCreate事务指定的FinishAfter时间在最近账本的关闭时间之后，则EscrowFinish交易将失败。
- 如果持有的付款有Condition，则除非您提供Fulfillment条件匹配，否则无法执行。
- 过期后您无法执行持有付款。具体来说，如果相应的EscrowCreate事务指定的CancelAfter时间在最近关账本的关闭时间之前，则EscrowFinish交易将失败。

**注意:** 如果包含fulfillment，则提交EscrowFinish交易的最低交易成本会增加。如果交易没有fulfillment，交易成本是标准的 10 drops。如果交易包含fulfillment，则交易成本为 330 drops SWTC加上每16字节加收 10 drops。

