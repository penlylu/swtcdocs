# 账本结构

## 账本示例

```json
{
    "ledger": {
        "accepted": true,
        "account_hash": "B2D2DD47534180CA42D52600B4380DE1A4ACAE028C818306784F923BC7D71043",
        "close_time": 592281040,
        "close_time_human": "2018-Oct-08 02:30:40",
        "close_time_resolution": 10,
        "closed": true,
        "hash": "382E786E3FE139DB0A3E9018E006DFABA192808D4A62068002CB079C70D3D3CC",
        "ledger_hash": "382E786E3FE139DB0A3E9018E006DFABA192808D4A62068002CB079C70D3D3CC",
        "ledger_index": "47467",
        "parent_hash": "4EAAF36DDC34F3365FC11D74D5980B68D58CF5245F3F02362579BA16588893AB",
        "seqNum": "47467",
        "totalCoins": "599999999999999316",
        "total_coins": "599999999999999316",
        "transaction_hash": "9EEE4ABB72D36CE9D86A6726A46CF029BCBEB9FFF9C65C2D3AA5263CD2C0046F",
        "transactions": [
            "2AA1A4C4950AF51BFE04AA6BE45E3F0CDC7CE28E6B442F7FB29EF61D2D8AE2EA"
        ]
    },
    "ledger_hash": "382E786E3FE139DB0A3E9018E006DFABA192808D4A62068002CB079C70D3D3CC",
    "ledger_index": 47467,
    "validated": true
}
```

## 属性说明

| 属性 | 类型 | 解释 |
|:-|:-|:-| 
| validated  | boolean | 为true时，为验证过的区块 | 
| ledger_hash | string | 除去ledger_hash外的区块的哈希值 |
| ledger_index | number | 该区块的序列号 | 
| ledger.accepted | boolean | 为true时，表示该区块正常关闭 |
| ledger.account_hash | string | 账户树的哈希值 |
| ledger.close_time | number | 该区块关闭的时间戳 |
| ledger.close_time_human | string | 该区块关闭的时间 |
| ledger.close_time_resolution | number | 关闭区块不同版本之间所差的秒数，在[2,120]之间 |
| ledger.closed | boolean | 为true时，为该区块最终关闭状态 |
| ledger.ledger_hash | string | 与ledger_hash相同 |
| ledger.ledger_index | string | 与ledger_index相同 |
| ledger.parent_hash | string | 上一个区块的的哈希值 |
| ledger.seqNum | string | 区块序列 |
| ledger.totalCoins | string | 除去消耗的swt, 剩下的swt总量 |
| ledger.total_coins | string | 同ledger.totalCoins |
| ledger.transaction_hash | string | 当前区块中所有交易的哈希值 |
| ledger.transactions | array<string> | 当前区块中所有交易的哈希值列表 |
