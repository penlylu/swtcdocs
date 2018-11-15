# SignerList

作为一个群体，SignerList对象类型代表各方，替代个人账户授权进行签名交易。您可以创建，替换或删除SignerListSet事务。此对象类型由MultiSign引入。

## 示例SignerList JSON


```json
{
    "Flags": 0,
    "LedgerEntryType": "SignerList",
    "OwnerNode": "0000000000000000",
    "PreviousTxnID": "5904C0DC72C58A83AEFED2FFC5386356AA83FCA6A88C89D00646E51E687CDBE4",
    "PreviousTxnLgrSeq": 16061435,
    "SignerEntries": [
        {
            "SignerEntry": {
                "Account": "jGyBtKKtrabR5VHbvKJ6wizUUBnrCWRTdw",
                "SignerWeight": 2
            }
        },
        {
            "SignerEntry": {
                "Account": "jLqM1U6Kp3BWCEsAvz8W1o7DhsQSA9GuPM",
                "SignerWeight": 1
            }
        },
        {
            "SignerEntry": {
                "Account": "jEoSyfChhUMzpRDttAJXuie8XhqyoPBYvV",
                "SignerWeight": 1
            }
        }
    ],
    "SignerListID": 0,
    "SignerQuorum": 3,
    "LedgerIndex": "A9C28A28B85CD533217F5C0A0C7767666B093FA58A0F2D80026FCC4CD932DDC7"
}
```

## SignerList字段
`SignerList`对象具有下列字段：

| 名称            | JSON 类型 | 类型 | 描述 |
|-----------------|-----------|---------------|-------------|
| `LedgerEntryType`   | String    | UInt16    | 账本对象类型。|
| `Flags`             | Number | UInt32 | 标志 |
| `PreviousTxnID`   | String    | Hash256       | 上一笔修改对象交易的哈希值。 |
| `PreviousTxnLgrSeq` | Number  | UInt32        | 上一笔修改对象交易所对应的账本索引。 |
| `OwnerNode`       | String    | UInt64        | 如果目录由多个页面组成，则提示所有者目录的哪个页面链接到此对象。 |
| `SignerEntries`   | Array     | Array         | 一组SignerEntry对象，表示属于此签名者列表的各方。 |
| `SignerListID`    | Number    | UInt32        | 此签名者列表的ID。目前始终设置为0。如果未来的修订允许帐户的多个签名者列表，则可能会更改。|
| `SignerQuorum`    | Number    | UInt32        | 签名者的数量。 |


### SignerEntry对象

该SignerEntries字段的每个成员都是一个描述列表中签名者的对象。SignerEntry具有以下字段：

| 名称            | JSON 类型 | 类型 | 描述 |
|-----------------|-----------|---------------|-------------|
| `Account`         | String    | AccountID     | 账户地址。它不需要是账本中的资金地址。 |
| `SignerWeight`    | Number    | UInt16        | 此签名者签名的权重。多签名仅在提供的签名的总权重超过SignerList的SignerQuorum值时才有效。|

如果地址与AccountRoot对象中的地址不对应，则只有与该地址关联的主密钥可用于生成有效签名。如果帐户配置了常规密钥，则可以使用常规密钥。帐户的主密钥只能在未禁用时使用。多签名不能用作另一个多签名的一部分。

## SignerLists and Reserves

SignerList本身计为两个对象，列表的每个成员都计为一个。因此，SignerList需要保留的swtc是账本中单个trustline或Offer对象所需保留的3倍到10倍。

