# DirectoryNode

DirectoryNode对象类型提供了一个账本的目录树，链接着其他对象。单个概念目录采用双向链表的形式，具有一个或多个DirectoryNode对象，每个对象最多包含32个其他对象的ID。第一个对象称为目录的根目录，可以根据需要添加或删除除根对象以外的所有对象。

目录有两种：

* **Owner directories** 列出了账户拥有的其他对象，如SkywellState或Offer对象。
* **Offer directories** 列出了可在多资产数字平台上交易的商品。单个商品目录包含具有相同发行相同汇率的所有商品。


## 示例 DirectoryNode JSON

<!-- MULTICODE_BLOCK_START -->

*Offer Directory*

```json
{
    "ExchangeRate": "4F069BA8FF484000",
    "Flags": 0,
    "Indexes": [
        "AD7EAE148287EF12D213A251015F86E6D4BD34B3C4A0A1ED9A17198373F908AD"
    ],
    "LedgerEntryType": "DirectoryNode",
    "RootIndex": "1BBEF97EDE88D40CEE2ADE6FEF121166AFE80D99EBADB01A4F069BA8FF484000",
    "TakerGetsCurrency": "0000000000000000000000000000000000000000",
    "TakerGetsIssuer": "0000000000000000000000000000000000000000",
    "TakerPaysCurrency": "0000000000000000000000004A50590000000000",
    "TakerPaysIssuer": "5BBC0F22F61D9224A110650CFE21CC0C4BE13098",
    "LedgerIndex": "1BBEF97EDE88D40CEE2ADE6FEF121166AFE80D99EBADB01A4F069BA8FF484000"
}
```

*Owner Directory*

```json
{
    "Flags": 0,
    "Indexes": [
        "AD7EAE148287EF12D213A251015F86E6D4BD34B3C4A0A1ED9A17198373F908AD",
        "E83BBB58949A8303DF07172B16FB8EFBA66B9191F3836EC27A4568ED5997BAC5"
    ],
    "LedgerEntryType": "DirectoryNode",
    "Owner": "jGyBtKKtrabR5VHbvKJ6wizUUBnrCWRTdw",
    "RootIndex": "193C591BF62482468422313F9D3274B5927CA80B4DD3707E42015DD609E39C94",
    "index": "193C591BF62482468422313F9D3274B5927CA80B4DD3707E42015DD609E39C94"
}
```

## DirectoryNode字段

| 名称              | JSON 类型 | 类型 | 描述 |
|-------------------|-----------|---------------|-------------|
| `LedgerEntryType`   | String    | UInt16    | 账本对象类型。 |
| `Flags`             | Number    | UInt32    | 标志，目前此字段对于DirectoryNode对象没有意义 |
| `RootIndex`         | String    | Hash256   | 此目录的根对象的ID。 |
| `Indexes`           | Array     | Vector256 | 此目录的内容：其他对象的ID数组。 |
| `IndexNext`         | Number    | UInt64    | (可选) 如果此目录由多个页面组成，则此ID链接到链中的下一个对象。 |
| `IndexPrevious`     | Number    | UInt64    | (可选) 如果此目录由多个页面组成，则此ID链接到链中的上一个对象。 |
| `Owner`             | String    | AccountID | (仅限所有者目录) 拥有此目录中对象帐户的地址。 |
| `ExchangeRate`      | Number    | UInt64    | （仅限提供目录）**已弃用**。不使用。 |
| `TakerPaysCurrency` | String    | Hash160   | （仅限提供目录）TakerPays的货币代码来自此目录中的商品。 |
| `TakerPaysIssuer`   | String    | Hash160   | （仅限提供目录）TakerPays的发行者。 |
| `TakerGetsCurrency` | String    | Hash160   | （仅限提供目录）TakerGets的货币代码来自此目录中的商品。 |
| `TakerGetsIssuer`   | String    | Hash160   | （仅限提供目录）TakerGets的发行者。 |
