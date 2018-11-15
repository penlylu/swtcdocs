# AccountRoot

`AccountRoot`对象类型描述了一个帐户。


## 示例AccountRoot JSON

```json
{
    "Account": "jHb9CJAWyB4jr91VRWn96DkukG4bwdtyTh",
    "AccountTxnID": "0D5FB50FA65C9FE1538FD7E398FFFE9D1908DFA4576D8D7A020040686F93C77D",
    "Balance": "148446663",
    "Domain": "6D64756F31332E636F6D",
    "EmailHash": "98B4375E1D753E5B91627516F6D70977",
    "Flags": 8388608,
    "LedgerEntryType": "AccountRoot",
    "MessageKey": "0000000000000000000000070000000300",
    "OwnerCount": 3,
    "PreviousTxnID": "0D5FB50FA65C9FE1538FD7E398FFFE9D1908DFA4576D8D7A020040686F93C77D",
    "PreviousTxnLgrSeq": 14091160,
    "Sequence": 336,
    "TransferRate": 1004999999,
    "LedgerIndex": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8"
}
```

## AccountRoot字段

`AccountRoot`对象具有以下字段：

| 字段                         | JSON类型 | 类型 | 描述  |
|:------------------------------|:----------|:------------------|:-------------|
| `LedgerEntryType`             | String    | UInt16            | `0x0061`匹配字符串`AccountRoot`,表明是一个`AccountRoot`对象 |
| `Account`                     | String    | AccountID         | 账户地址，例如jHb9CJAWyB4jr91VRWn96DkukG4bwdtyTh。 |
| `Balance`                     | String    | Amount            | 余额，单位为`drop`|
| `Flags` | Number    | UInt32            | 账户的标识 |
| `OwnerCount`                  | Number    | UInt32            | 账户对象的个数 |
| `PreviousTxnID`               | String    | Hash256           | 上一笔修改对象交易的哈希值 |
| `PreviousTxnLgrSeq`           | Number    | UInt32            | 上一笔修改对象交易所对应的账本索引 |
| `Sequence`                    | Number    | UInt32            | 序列号 (从1开始，随着交易数增加) |
| `AccountTxnID`                | String    | Hash256           | _(可选)_ 交易哈希值 |
| `Domain`                      | String    | VariableLength    | _(可选)_ 域名 |
| `EmailHash`                   | String    | Hash128           | _(可选)_ 邮件的MD5哈希值 |
| `MessageKey`                  | String    | VariableLength    | _(可选)_ 加密信息，使用十六进制值，不超过33字节 |
| `RegularKey`                  | String    | AccountID         | _(可选)_ 常规密钥，可以代替主密钥签名|
| `TransferRate`                | Number    | UInt32            | _(可选)_ 交易费率 |
| `WalletLocator`               | String    | Hash256           | _(可选)_ **弃用**. |
| `WalletSize`                  | Number    | UInt32            | _(可选)_ **弃用**. |

## AccountRoot标志

可以为帐户启用或禁用多个选项。可以使用AccountSet事务更改这些选项。在分类帐中，标志表示为二进制值，可以与按位或操作组合。分类帐中标志的位值与用于在事务中启用或禁用这些标志的值不同。分类帐标志的名称以lsf开头。

AccountRoot对象可以具有以下标志值：

| 标志名称 | 十六进制值 | 十进制 | 描述 | 对应的标志(accountset.html#accountset-flags) |
|-----------|-----------|---------------|-------------|-------------------------------|
| lsfDefaultSkywell | 0x00800000 | 8388608 | trust lines默认允许SWTC | asfDefaultSkywell |
| lsfDisableMaster | 0x00100000 | 1048576 | 不允许使用主密钥签名交易 |
| lsfDisallowSWT | 0x00080000 | 524288 | 客户端应用不能发送SWTC到此账户 | asfDisallowSWT |
| lsfGlobalFreeze | 0x00400000 | 4194304 |　这个账户的所有资产都被冻结 | asfGlobalFreeze |
| lsfNoFreeze | 0x00200000 | 2097152 | 这个地址不能冻结trust lines | asfNoFreeze |
| lsfPasswordSpent | 0x00010000 | 65536 | 设置密码的费用已支付 | (None) |
| lsfRequireAuth | 0x00040000 | 262144 | 允许用户发行 | asfRequireAuth |
| lsfRequireDestTag | 0x00020000 | 131072 | 需要为Destination Tag支付 | asfRequireDest |

