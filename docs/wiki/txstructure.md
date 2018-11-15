# 交易结构

***

## 交易示例

```json
{
    "Account": "jQfvETFtVeQGXQZbMTxUm5VHgSA145ucPR",
    "Amount": {
	    "currency": "8E17145E33EB0B9BFFBEC39AC9B935BF51127214",
	    "issuer": "jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or",
	    "value": "1"
	},
    "Destination": "jw27f8oUXpJtM4YeATNE9ozSrzFRRxG6R5",
    "Fee": "12",
    "Flags": 2147483648,
    "Memos":[
	    {
	        "Memo": {
	            "MemoData": "7968",
	            "MemoType": "537472696E67"
	        }
	    }
	],
    "Sequence": 2,
    "SigningPubKey": "029AE12D29CE8C5A2A1D693C8155FFCFE2E56423C30CD175AA3241F35F19733F34",
    "TransactionType": "Payment",
    "TxnSignature": "3044022057642B1066DE36CE676B0FE7BDF6CE35E0744FE954126F479BA7C962D4BB941D0220480D6059B85B44FC3CCB070982C5A45E3AE359534C00BCAEFC8B56936B3D47D2",
    "date": 592281040,
    "hash": "2AA1A4C4950AF51BFE04AA6BE45E3F0CDC7CE28E6B442F7FB29EF61D2D8AE2EA",
    "inLedger": 47467,
    "ledger_hash": "382E786E3FE139DB0A3E9018E006DFABA192808D4A62068002CB079C70D3D3CC",
    "ledger_index": 47467,
    "meta": {
	    "AffectedNodes": [
	        {
	            "ModifiedNode": {
	                "FinalFields": {
	                    "Balance": {
	                        "currency": "8E17145E33EB0B9BFFBEC39AC9B935BF51127214",
	                        "issuer": "jjjjjjjjjjjjjjjjjjjjBZbvri",
	                        "value": "-513.5373855"
	                    },
	                    "Flags": 2228224,
	                    "HighLimit": {
	                        "currency": "8E17145E33EB0B9BFFBEC39AC9B935BF51127214",
	                        "issuer": "jMkda2zVYe56aCHksra7D6o3KFBrX8VPsa",
	                        "value": "10000000000"
	                    },
	                    "HighNode": "0000000000000000",
	                    "LowLimit": {
	                        "currency": "8E17145E33EB0B9BFFBEC39AC9B935BF51127214",
	                        "issuer": "jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or",
	                        "value": "0"
	                    },
	                    "LowNode": "0000000000000D2A"
	                },
	                "LedgerEntryType": "SkywellState",
	                "LedgerIndex": "06787C88900154D72D15257287D4B4C55E9B21A69A5C4FDE41D01B2D85C8D520",
	                "PreviousFields": {
	                    "Balance": {
	                        "currency": "8E17145E33EB0B9BFFBEC39AC9B935BF51127214",
	                        "issuer": "jjjjjjjjjjjjjjjjjjjjBZbvri",
	                        "value": "-513.5369342"
	                    }
	                },
	                "PreviousTxnID": "E0172B00FD1E3E82DD1C1D7B7EFA5FF1E9688683A2029BC1789F4D00BFF5C45B",
	                "PreviousTxnLgrSeq": 7107487
	            }
	        },
	        {
	            "ModifiedNode": {
	                "FinalFields": {
	                    "Account": "jEoSyfChhUMzpRDttAJXuie8XhqyoPBYvV",
	                    "Balance": "133094777",
	                    "Flags": 0,
	                    "OwnerCount": 0,
	                    "Sequence": 2764370
	                },
	                "LedgerEntryType": "AccountRoot",
	                "LedgerIndex": "109E80FB8CC6D82D4F7F7D77248C2C3C116ECCD4520B3D2A88421FFF94A57B1E",
	                "PreviousFields": {
	                    "Balance": "133094765",
	                    "Sequence": 2764369
	                },
	                "PreviousTxnID": "1D58CA8A99BC1D73EEA81776EE07907FD0F25E759B66F25BB86983ECF21A4EFF",
	                "PreviousTxnLgrSeq": 7107487
	            }
	        },
	        {
	            "ModifiedNode": {
	                "FinalFields": {
	                    "Account": "jNoNeVSSWuY4NZhNrsA6Fk4M7bRMTes5yN",
	                    "Balance": "66178441678",
	                    "Flags": 0,
	                    "OwnerCount": 1,
	                    "Sequence": 2149171
	                },
	                "LedgerEntryType": "AccountRoot",
	                "LedgerIndex": "AABC7A73515F8F42D465BCF177B2E39E07361E12612E4F9FEB18C248E0607494",
	                "PreviousFields": {
	                    "Balance": "66178441690",
	                    "Sequence": 2149170
	                },
	                "PreviousTxnID": "1D58CA8A99BC1D73EEA81776EE07907FD0F25E759B66F25BB86983ECF21A4EFF",
	                "PreviousTxnLgrSeq": 7107487
	            }
	        },
	        {
	            "ModifiedNode": {
	                "FinalFields": {
	                    "Balance": {
	                        "currency": "8E17145E33EB0B9BFFBEC39AC9B935BF51127214",
	                        "issuer": "jjjjjjjjjjjjjjjjjjjjBZbvri",
	                        "value": "858022.7271187"
	                    },
	                    "Flags": 1114112,
	                    "HighLimit": {
	                        "currency": "8E17145E33EB0B9BFFBEC39AC9B935BF51127214",
	                        "issuer": "jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or",
	                        "value": "0"
	                    },
	                    "HighNode": "0000000000000CC9",
	                    "LowLimit": {
	                        "currency": "8E17145E33EB0B9BFFBEC39AC9B935BF51127214",
	                        "issuer": "jNoNeVSSWuY4NZhNrsA6Fk4M7bRMTes5yN",
	                        "value": "10000000000"
	                    },
	                    "LowNode": "0000000000000000"
	                },
	                "LedgerEntryType": "SkywellState",
	                "LedgerIndex": "E41D3F2F55927AFE28A5AB0253F68D2F8FD4E8767090E28C1BD0F5001D4480E1",
	                "PreviousFields": {
	                    "Balance": {
	                        "currency": "8E17145E33EB0B9BFFBEC39AC9B935BF51127214",
	                        "issuer": "jjjjjjjjjjjjjjjjjjjjBZbvri",
	                        "value": "858022.72757"
	                    }
	                },
	                "PreviousTxnID": "1D58CA8A99BC1D73EEA81776EE07907FD0F25E759B66F25BB86983ECF21A4EFF",
	                "PreviousTxnLgrSeq": 7107487
	            }
	        }
	    ],
	    "TransactionIndex": 22,
	    "TransactionResult": "tesSUCCESS",
	    "hash": "05E1678F152A97BCA616A9A1E37EE15566DC2D8C8B9CD7AE80C9E045FA7640D8"
	},
    "validated": true
}
```

## 属性说明

| 属性 | 类型 | 解释 |
|:-|:-|:-| 
| Account | string | 交易发起方的账户地址 |
| Amount | string | swt的转账数量 |
| Amount | object | 银关发行的代币转账数量 |
| Amount.currency | string | 转账代币的名称 |
| Amount.issuer | string | 转账代币的银关地址 |
| Amount.currency | string | 转账代币的数量 |
| Destination | string | 交易接收方的账户地址 |
| Fee | string | 转账费用 |
| Memos | array | 交易备注 |
| Memos[].MemoData | string | 备注项的内容 |
| Memos[].MemoType | string | 备注项的类型 |
| Sequence | number | 交易发起账户的发起交易的编号 |
| SigningPubKey | string | 交易发起账户的公钥 |
| Timestamp | number | 交易发起的时间戳 |
| TransactionType | string | 交易类型 |
| TxnSignarture | string | 交易签名 |
| date | string | 交易的时间戳 |
| hash | string | 交易的哈希值 |
| inLedger | number | 交易所在的区块号 |
| ledger_hash | string | 交易所在的区块哈希值 |
| validated | boolean | 为true时，该交易为验证过的交易 |
| meta | object | 交易执行结果 |
| meta.AffectedNodes | array | 交易影响到的相关账户 |
| meta.AffectedNodes.ModifiedNode | object | 交易影响到的账户 |
| meta.AffectedNodes.ModifiedNode.FinalFields | object | 交易影响到的账户的最终变动情况 |
| meta.AffectedNodes.ModifiedNode.PreviousFields | object | 交易影响到的账户的该变动的上一个历史情况 |
| TransactionIndex | number | 交易在区块里的下标 |
| TransactionResult | string | 交易执行结果的状态 |
