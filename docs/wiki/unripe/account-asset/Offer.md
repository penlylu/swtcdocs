# Offer

`Offer`对象类型描述了交换货币的挂单交易。当挂单交易没有立刻成交，一个OfferCreate交易就会创建Offer对象。

## 示例Offer JSON

```json
{
    "Account": "j3N35VHut94dD1Y9H1KoWmGZE2kNNRFcVk",
    "BookDirectory": "ACC27DE91DBA86FC509069EAF4BC511D73128B780F2E54BF5E07A369E2446000",
    "BookNode": "0000000000000000",
    "Flags": 131072,
    "LedgerEntryType": "Offer",
    "OwnerNode": "0000000000000000",
    "PreviousTxnID": "F0AB71E777B2DA54B86231E19B82554EF1F8211F92ECA473121C655BFC5329BF",
    "PreviousTxnLgrSeq": 14524914,
    "Sequence": 866,
    "TakerGets": {
        "currency": "BTC",
        "issuer": "jBciDE8Q3uJjf111VeiUNM775AMKHEbBLS",
        "value": "37"
    },
    "TakerPays": "79550000000",
    "LedgerIndex": "96F76F27D8A327FC48753167EC04A46AA0E382E6F57F32FD12274144D00F1797"
}
```

## Offer 字段

一个`Offer`对象具有以下字段：

| 名称              | JSON 类型 | 类型 | 描述 |
|-------------------|-----------|---------------|-------------|
| `LedgerEntryType`   | String    | UInt16    | 账本对象类型。 |
| `Flags`             | Number    | UInt32    | 标志，见下表。 |
| `Account`           | String    | AccountID | 账户地址。 |
| `Sequence`          | Number    | UInt32    | 序列号(从1开始，随着交易数增加) 。 |
| `TakerPays`         | `Offer`创建者要求的剩余金额和货币类型。|
| `TakerGets`         | `Offer`创建者提供的剩余金额和货币类型。|
| `BookDirectory`     | String    | UInt256   | 连接到此`Offer`的[DirectoryNode](DirectoryNode.md)的ID 。 |
| `BookNode`          | String    | UInt64    | 如果目录由多个页面组成，则提示`Offer`目录的哪个页面链接到此对象。 |
| `OwnerNode`         | String    | UInt64    | 如果目录由多个页面组成，则提示所有者目录的哪个页面链接到此对象。 |
| `PreviousTxnID`     | String | Hash256 | 上一笔修改对象交易的哈希值。 |
| `PreviousTxnLgrSeq` | Number | UInt32 | 上一笔修改对象交易所对应的账本索引。 |
| `Expiration`        | Number    | UInt32    | (可选) 过期时间 |

## Offer标志

当OfferCreate事务创建`Offer`对象时，可以启用或禁用多个选项。在账本中，标志表示为二进制值，可以与按位或操作组合。账本中标志的位值与用于在事务中启用或禁用这些标志的值不同。账本标志的名称以 _lsf_ 开头。

`Offer`对象可以具有以下标志值：

| 标志名称 | 十六进制值 | 十进制值 | 描述 | 相应的OfferCreate标志 |
|-----------|-----------|---------------|-------------|------------------------|
| lsfPassive | 0x00010000 | 65536 | 该对象被视为被动的offer。这对账本中的对象没有影响。 | tfPassive |
| lsfSell   | 0x00020000 | 131072 | 该对象被作为卖出Offer。这对分类帐中的对象没有影响。 | tfSell |
