# Service节点手册

## 主要功能

与 public node 相连，提供所有的账本的实时同步功能；提供一套全新的访问接口，可以对外提供SWTC公链相关的API与RPC调用。

## 命令行参数

	- 参数配置类
		* **-data_dir**， 指定帐本存放目录，默认为当前目录
		* **-database_name**， 指定帐本数据库目录名，默认为"jingtum.db"
		* **-endpoint**, 指定连接的public node的地址，默认为测试环境的一个public node的地址，即"ws://ts.jingtum.com:5020"
		* **-thread_count**, 指定同步帐本的线程数量，默认为8
	- 帐户管理类
		* **account list**， 列出本地帐户相关情况
		* **account generate [-nickname]**， 生成新的本地帐户，可指定新帐户的nickname
	- 启动服务类
		* **-sync**, 启动帐本实时同步功能，默认为true
		* **-api**, 启动JSON REST API服务，默认为false
		* **-apiaddr**, 设定启动的JSON REST API服务的地址，默认为"localhost"
		* **-apiport**, 设定启动的JSON REST API服务的端口，默认为7544
		* **-rpc**, 启动JSON RPC API服务，默认为false
		* **-rpcaddr**, 设定启动的JSON RPC API服务的地址，默认为"localhost"
		* **-rpcport**, 设定启动的JSON RPC API服务的端口，默认为7545

- [JSON RPC API](#json-rpc-api)
	- [JSON-RPC methods](#json-rpc-methods)
		* [jt_syncing](#jt_syncing)
		* [jt_blockNumber](#jt_blocknumber)
		* [jt_getBalance](#jt_getbalance)
		* [jt_getTransactionCount](#jt_gettransactioncount)
		* [jt_getBlockTransactionCountByHash](#jt_getblocktransactioncountbyhash)
		* [jt_getBlockTransactionCountByNumber](#jt_getblocktransactioncountbynumber)
		* [jt_getBlockByHash](#jt_getblockbyhash)
		* [jt_getBlockByNumber](#jt_getblockbynumber)
		* [jt_getTransactionByHash](#jt_gettransactionbyhash)
		* [jt_getTransactionByBlockHashAndIndex](#jt_gettransactionbyblockhashandindex)
		* [jt_getTransactionByBlockNumberAndIndex](#jt_gettransactionbyblocknumberandindex)
		* [jt_getTransactionReceipt](#jt_gettransactionreceipt)
		* [jt_accounts](#jt_accounts)
		* [jt_sign](#jt_sign)
		* [jt_sendTransaction](#jt_sendtransaction)
		* [jt_signTransaction](#jt_signtransaction)
		* [jt_sendRawTransaction](#jt_sendrawtransaction)
		* [jt_account](#jt_account)
		- [JSON RPC API Reference](#json-rpc-api-reference) 

## JSON RPC API Reference

***

#### jt_syncing

返回同步状态数据对象或`false`


##### Parameters 参数

无

##### Returns 返回值

`Object|Boolean`, 一个带有同步状态数据的对象。当未同步时，返回结果为`FALSE`:  
  - `startingBlock`: `QUANTITY` - import指令开始的区块（仅在同步信号到达其顶部时会被重置）  
  - `currentBlock`: `QUANTITY` - 当前区块, 同 jt_blockNumber  
  - `highestBlock`: `QUANTITY` - 预估最高区块

##### Example 举例
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_syncing","params":[],"id":1}' http://localhost:7545/v1/jsonrpc

// Result 结果
{
  "id":1,
  "jsonrpc":"2.0",
  "result":
    {
      "currentBlock":4599,
      "highestBlock":850088,
      "startingBlock":20
    },
  "status":"success"
}

// Or when not syncing 未同步时的结果
{
  "id":1,
  "jsonrpc": "2.0",
  "result": false,
  "status": "success"
}
```

***

#### jt_blockNumber

返回最新区块的编号

##### Parameters 参数
无

##### Returns 返回值

`QUANTITY` - 客户端运行的当前区块编号.

##### Example 例子
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_blockNumber","params":[],"id":1}' http://localhost:7545/v1/jsonrpc

// Result 结果
{
  "id":1,
  "jsonrpc": "2.0",
  "result": 112948,
  "status": "success"
}
```

***

#### jt_getBalance

返回给定地址的帐户余额

##### Parameters 参数

1. `DATA` - 所要查询余额的地址.
2. `QUANTITY|TAG` - 区块编号, 区块哈希, 或者是字符串 `"validated"`, `"current"`, `"closed"`中的一个, 详见 [default block parameter](#the-default-block-parameter)

```js
params: [
   "j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E",
   "validated"
]
```

##### Returns 返回值

`OBJECT` - 当前余额和货币列表


##### Example 例子
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_getBalance","params":["j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E", "validated"],"id":1}' http://localhost:7545/v1/jsonrpc

// Result 结果
{
  "id":1,
  "jsonrpc":"2.0",
  "result":
{
	"account":"j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E",
	"balance":"82002267",
	"coins":[
		{
			"account":"jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or",
			"balance":"0.02",
			"currency":"CNY",
			"limit":"10000000000",
			"limit_peer":"0",
			"no_skywell":true,
			"quality_in":0,
			"quality_out":0
		}
	],
  "ledger_hash":"6ACD74B8C7E0E9E9DE001BD98DCCA45F26CC47BE455C681CB32440BD1BF1991D",
  "ledger_index":10449036,
  "validated":true
  },
  "status":"success"
}
```

***

#### jt_getTransactionCount

返回某个地址*sent*（发起）的交易数


##### Parameters 参数

1. `DATA` - 地址.
2. `QUANTITY|TAG` - 区块编号, 区块哈希, 或者是字符串 `"validated"`, `"current"`, `"closed"`中的一个, 详见 [default block parameter](#the-default-block-parameter)

```js
params: [
   'j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E',
   '113199' // the block number 区块编号
]
```

##### Returns 返回值

`QUANTITY` - 该地址发起的交易数.


##### Example 例子
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_getTransactionCount","params":["j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E", "113199"],"id":1}' http://localhost:7545/v1/jsonrpc

// Result 结果
{
	"id":1,
	"jsonrpc":"2.0","
	result":1,
	"status":"success"
}
```

***

#### jt_getBlockTransactionCountByHash

返回给定哈希值所对应的区块的交易数


##### Parameters 参数

1. `DATA` - 区块哈希

```js
params: [
   '4F8040D0EF9A2360DFE5E715D3991B75340DA937F5ABA00C7686B76B1F84E26E'
]
```

##### Returns 返回值

`QUANTITY` - 这个区块的交易数.


##### Example 例子
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_getBlockTransactionCountByHash","params":["4F8040D0EF9A2360DFE5E715D3991B75340DA937F5ABA00C7686B76B1F84E26E"],"id":1}' http://localhost:7545/v1/jsonrpc

// Result 结果
{
	"id":1,
	"jsonrpc":"2.0",
	"result":2,
	"status":"success"
}
```

***

#### jt_getBlockTransactionCountByNumber

返回给定编号所对应的区块的交易数

##### Parameters 参数

1. `QUANTITY|TAG` - 区块编号

```js
params: [
   '0xe8', // 232
]
```

##### Returns 返回值

`QUANTITY` - 这个区块的交易数.

##### Example 例子
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_getBlockTransactionCountByNumber","params":["113199"],"id":1}' http://localhost:7545/v1/jsonrpc

// Result 结果
{
  "id":1,
  "jsonrpc": "2.0",
  "result":0,
  "status":"success"
}
```

***

#### jt_getBlockByHash

返回哈希值所对应的区块信息

##### Parameters 参数

1. `DATA`, 32 Bytes - 区块哈希
2. `Boolean` - 如果参数为true`，则返回整个交易对象，如果参数为`false`则只返回交易哈希。

```js
params: [
   "4F8040D0EF9A2360DFE5E715D3991B75340DA937F5ABA00C7686B76B1F84E26E",
   true
]
```

##### Returns 返回值

`Object` - 一个区块对象，当区块不存在时则返回`null`:


##### Example 例子
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_getBlockByHash","params":["4F8040D0EF9A2360DFE5E715D3991B75340DA937F5ABA00C7686B76B1F84E26E", false],"id":1}' http://localhost:7545/v1/jsonrpc

// Result 结果
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "ledger": {
      "accepted": true,
      "account_hash": "63A773D95DA86938313CEB400ADC90A6AED4C087985755E1F3596041A5226258",
      "close_time": 587650500,
      "close_time_human": "2018-Aug-15 12:15:00",
      "close_time_resolution": 10,
      "closed": true,
      "hash": "4F8040D0EF9A2360DFE5E715D3991B75340DA937F5ABA00C7686B76B1F84E26E",
      "ledger_hash": "4F8040D0EF9A2360DFE5E715D3991B75340DA937F5ABA00C7686B76B1F84E26E",
      "ledger_index": "113199",
      "parent_hash": "144C61BA96372BA427DE2C1244BD8D8F6E18810E403999599F0AC9E111E8D220",
      "seqNum": "113199",
      "totalCoins": "600000000000000000",
      "total_coins": "600000000000000000",
      "transaction_hash": "0000000000000000000000000000000000000000000000000000000000000000",
      "transactions": [
		  {
		    "Account": "jHb9CJAWyB4jr91VRWn96DkukG4bwdtyTh",
		    "Amount": "1",
		    "Destination": "j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E",
		    "Fee": "12",
		    "Flags": 2147483648,
		    "Sequence": 362,
		    "SigningPubKey": "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020",
		    "TransactionType": "Payment",
		    "TxnSignature": "3045022100EAAE8B2E451D784CEE5406BBFE4FDA286152AC3395F672AF09689D2F85C4510D02202EF038B5DF3088A2AC097EF1617103BDDF2513FFA11D8E5E868C6465BDDADBCC",
		    "date": 587199710,
		    "hash": "0019A03D4D5CAC27A2B25F3E2BEBBE2B0FE68C758128481E7D50FFD2D12C6F55",
		    "inLedger": 68120,
		    "ledger_hash": "3B98F9DB9BA4A91A140C5ACC8E6FAA01746408938E89F7D1963AEB257228DBBA",
		    "ledger_index": 68120,
		    "validated": true
		  }
      ]
    },
    "ledger_hash": "4F8040D0EF9A2360DFE5E715D3991B75340DA937F5ABA00C7686B76B1F84E26E",
    "ledger_index": 113199,
    "validated": true
  },
  "status": "success"
}
```

***

#### jt_getBlockByNumber

返回区块编号所对应的区块信息

##### Parameters 参数

1. `QUANTITY|TAG` - 区块编号，或者是字符串`"earliest"`, `"latest"`, `"pending"`中的一个, 正如在 [default block parameter](#the-default-block-parameter)里的一样.
2. `Boolean` - 如果参数为true`，则返回整个交易对象，如果参数为`false`则只返回交易哈希。

```js
params: [
	"113199",
   true
]
```

##### Returns 返回值

详见 [jt_getBlockByHash](#jt_getblockbyhash)

##### Example 例子
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_getBlockByNumber","params":["113199", false],"id":1}' http://localhost:7545/v1/jsonrpc
```

结果详见 [jt_getBlockByHash](#jt_getblockbyhash)

***

#### jt_getTransactionByHash

返回交易哈希请求的交易信息


##### Parameters 参数

1. `DATA`, 32 Bytes - 交易哈希

```js
params: [
   "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"
]
```

##### Returns 返回值

`Object` - 返回一个交易对象，当交易不存在时返回`null`

##### Example 例子
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_getTransactionByHash","params":["0019A03D4D5CAC27A2B25F3E2BEBBE2B0FE68C758128481E7D50FFD2D12C6F55"],"id":1}' http://localhost:7545/v1/jsonrpc

// Result 结果
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "Account": "jHb9CJAWyB4jr91VRWn96DkukG4bwdtyTh",
    "Amount": "1",
    "Destination": "j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E",
    "Fee": "12",
    "Flags": 2147483648,
    "Sequence": 362,
    "SigningPubKey": "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020",
    "TransactionType": "Payment",
    "TxnSignature": "3045022100EAAE8B2E451D784CEE5406BBFE4FDA286152AC3395F672AF09689D2F85C4510D02202EF038B5DF3088A2AC097EF1617103BDDF2513FFA11D8E5E868C6465BDDADBCC",
    "date": 587199710,
    "hash": "0019A03D4D5CAC27A2B25F3E2BEBBE2B0FE68C758128481E7D50FFD2D12C6F55",
    "inLedger": 68120,
    "ledger_hash": "3B98F9DB9BA4A91A140C5ACC8E6FAA01746408938E89F7D1963AEB257228DBBA",
    "ledger_index": 68120,
    "validated": true
  },
  "status": "success"
}
```

***

#### jt_getTransactionByBlockHashAndIndex

返回区块哈希和交易索引所对应的交易信息


##### Parameters 参数

1. `DATA`, 32 Bytes - 区块哈希.
2. `QUANTITY` - 交易索引.

```js
params: [
   "3B98F9DB9BA4A91A140C5ACC8E6FAA01746408938E89F7D1963AEB257228DBBA",
   "0"
]
```

##### Returns 返回值

详见 [jt_getTransactionByHash](#jt_gettransactionbyhash)

##### Example 例子
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_getTransactionByBlockHashAndIndex","params":["3B98F9DB9BA4A91A140C5ACC8E6FAA01746408938E89F7D1963AEB257228DBBA", "0"],"id":1}' http://localhost:7545/v1/jsonrpc
```

结果详见 [jt_getTransactionByHash](#jt_gettransactionbyhash)

***

#### jt_getTransactionByBlockNumberAndIndex

返回区块编号和交易索引所对应的交易信息.


##### Parameters 参数

1. `QUANTITY|TAG` - 区块编号，或者是字符串`"earliest"`, `"latest"`, `"pending"`中的一个, 正如在 [default block parameter](#the-default-block-parameter)里的一样.
2. `QUANTITY` - 交易索引.

```js
params: [
   "68120",
   "0"
]
```

##### Returns 返回值

详见 [jt_getTransactionByHash](#jt_gettransactionbyhash)

##### Example 例子
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_getTransactionByBlockNumberAndIndex","params":["68120", "0"],"id":1}' http://localhost:7545/v1/jsonrpc
```

结果详见 [jt_getTransactionByHash](#jt_gettransactionbyhash)

***

#### jt_getTransactionReceipt

返回交易哈希所对应的交易收据.

**注意** 无法获得正在进行中的交易的收据.


##### Parameters 参数

1. `DATA`, 32 Bytes - 交易哈希

```js
params: [
   "0019A03D4D5CAC27A2B25F3E2BEBBE2B0FE68C758128481E7D50FFD2D12C6F55"
]
```

##### Returns 返回值

`Object` - 返回一个交易收据对象，当收据不存在时返回`null`


##### Example 例子
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_getTransactionReceipt","params":["0019A03D4D5CAC27A2B25F3E2BEBBE2B0FE68C758128481E7D50FFD2D12C6F55"],"id":1}' http://localhost:7545/v1/jsonrpc

// Result 结果
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "AffectedNodes": [
      {
        "ModifiedNode": {
          "FinalFields": {
            "Account": "jHb9CJAWyB4jr91VRWn96DkukG4bwdtyTh",
            "Balance": "244499999099995309",
            "Flags": 0,
            "OwnerCount": 0,
            "Sequence": 363
          },
          "LedgerEntryType": "AccountRoot",
          "LedgerIndex": "2B6AC232AA4C4BE41BF49D2459FA4A0347E1B543A4C92FCEE0821C0201E2E9A8",
          "PreviousFields": {
            "Balance": "244499999099995322",
            "Sequence": 362
          },
          "PreviousTxnID": "79EA2CCE7B64AADCC02CB252AE3E2E375BBEEB11A61A16B2634C4CDE54EA74CE",
          "PreviousTxnLgrSeq": 68120
        }
      },
      {
        "ModifiedNode": {
          "FinalFields": {
            "Account": "j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E",
            "Balance": "200000346",
            "Flags": 0,
            "OwnerCount": 0,
            "Sequence": 1
          },
          "LedgerEntryType": "AccountRoot",
          "LedgerIndex": "BA84D071F8AB10AA7FBA28E04EF1E75A2D36362B85BFC494FCF257C5960C8A09",
          "PreviousFields": {
            "Balance": "200000345"
          },
          "PreviousTxnID": "79EA2CCE7B64AADCC02CB252AE3E2E375BBEEB11A61A16B2634C4CDE54EA74CE",
          "PreviousTxnLgrSeq": 68120
        }
      },
      {
        "ModifiedNode": {
          "FinalFields": {
            "Account": "jBbRz4Erf7j4oBboXcChKWVgX5HhymygLT",
            "Balance": "4332",
            "Flags": 0,
            "OwnerCount": 0,
            "Sequence": 362
          },
          "LedgerEntryType": "AccountRoot",
          "LedgerIndex": "F88E3CB9279ED427BB4CF37D13840A0728489CC273AE92A5C05D5390B1FF5F2A",
          "PreviousFields": {
            "Balance": "4320",
            "Sequence": 361
          },
          "PreviousTxnID": "79EA2CCE7B64AADCC02CB252AE3E2E375BBEEB11A61A16B2634C4CDE54EA74CE",
          "PreviousTxnLgrSeq": 68120
        }
      }
    ],
    "TransactionIndex": 249,
    "TransactionResult": "tesSUCCESS",
    "hash": "0019A03D4D5CAC27A2B25F3E2BEBBE2B0FE68C758128481E7D50FFD2D12C6F55"
  },
  "status": "success"
}
```

***

#### jt_accounts

返回客户端所拥有的地址列表.


##### Parameters 参数
无

##### Returns 返回值

`Array of DATA`, 20 Bytes - 客户端所拥有的所有地址

##### Example 例子
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_accounts","params":[],"id":1}' http://localhost:7545/v1/jsonrpc

// Result 结果
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": [
    "jPtkTgG9o4Jfq6w5G6FaNGgEnVRsSNoUuB",
    "jfRiTK5t9uBcmfYp5B95q5k3oggdwGLcSv",
    "j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E"
  ],
  "status": "success"
}
```

***

#### jt_sign

通过给消息添加前缀，使得计算出的签名可以被识别成Skywell特定签名.

##### Parameters 参数
账户，信息

1. `DATA` - 地址
2. `DATA`, N Bytes - 要签名的消息

##### Returns 返回值

`DATA`: 签名

##### Example 例子

```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_sign","params":["j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E","0xdeadbeaf"],"id":1}' http://localhost:7545/v1/jsonrpc

// Result 结果
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": "0x30450221008D119BA6ABB97D5A541D0BD5649AA0DC35E00C19E5C381A15611CD38E933B18902201E76D713F7966A1F1BAC6EFB87D0953F8697EB6DFE485F434F34D959ACD74FED",
  "status": "success"
}
```

***

#### jt_sendTransaction

创建一个新交易，签名，并提交交易.

##### Parameters 参数

1. `Object` - 交易对象
  - `from`: `DATA` - 交易发起一方的地址
  - `to`: `DATA` - (当创建新交易时可选) 交易所指向的地址。
  - `fee`: `QUANTITY`  - (可选，默认12) 执行交易所需提供的交易费用. 
  - `value`: `QUANTITY`  - (可选) 整数交易金额.

```js
params: [
	{
		"from":"jHb9CJAWyB4jr91VRWn96DkukG4bwdtyTh",
		"to":"j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E",
		"value":"1"
	}
]
```

##### Returns 返回值

`DATA`, 32 Bytes - 返回交易哈希，当交易不存在时返回0哈希.

使用 [jt_getTransactionReceipt](#jt_gettransactionreceipt) 来获取交易收据

##### Example 例子
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_sendTransaction","params":[{"from":"jHb9CJAWyB4jr91VRWn96DkukG4bwdtyTh","to":"j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E","value":"1"}],"id":1}' http://localhost:7545/v1/jsonrpc

// Result 结果
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": [
    "637FE35E7ECEDA862A1C51596945F69444C169796197CC174F449C176116395D"
  ],
  "status": "success"
}
```

***

#### jt_signTransaction

创建并签署一个交易

##### Parameters 参数

1. `Object` - 交易对象
  - `from`: `DATA` - 交易发起一方的地址
  - `to`: `DATA` - (当创建新交易时可选) 交易所指向的地址。
  - `fee`: `QUANTITY`  - (可选，默认12) 执行交易所需提供的交易费用. 
  - `value`: `QUANTITY`  - (可选) 整数交易金额.

```js
params: [
	{
		"from":"jHb9CJAWyB4jr91VRWn96DkukG4bwdtyTh",
		"to":"j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E",
		"value":"1"
	}
]
```

##### Returns 返回值

`DATA` - 十六进制的签名交易.

##### Example 例子
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_signTransaction","params":[{"from":"jHb9CJAWyB4jr91VRWn96DkukG4bwdtyTh","to":"j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E","value":"1"}],"id":1}' http://localhost:7545/v1/jsonrpc

// Result 结果
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": [
    "120000228000000024000003FB61400000000000000168400000000000000C73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD0207446304402207268AD2FC68CD6AB8157F5BD0B0884F37DFF18D01E5414D6A277E33D4328D2BD02205502FDFFF00B7F1F9D7EB75329F65B50E619B6E1F9D24BE0D65F862419398A178114B5F762798A53D543A014CAF8B297CFF8F2F937E883145D2221763934C9380F43168B760199AC8C4DC074"
  ],
  "status": "success"
}
```

***

#### jt_sendRawTransaction

提交十六进制格式的原始交易

##### Parameters 参数

1. `DATA`, 签署后的交易数据.

```js
params: ["120000228000000024000003FB61400000000000000168400000000000000C73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD0207446304402207268AD2FC68CD6AB8157F5BD0B0884F37DFF18D01E5414D6A277E33D4328D2BD02205502FDFFF00B7F1F9D7EB75329F65B50E619B6E1F9D24BE0D65F862419398A178114B5F762798A53D543A014CAF8B297CFF8F2F937E883145D2221763934C9380F43168B760199AC8C4DC074"]
```

##### Returns 返回值

`DATA` - 返回交易哈希，当交易不存在时返回0哈希.

在交易生效后，使用 [jt_getTransactionReceipt](#jt_gettransactionreceipt) 来获取交易收据

##### Example 例子
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_sendRawTransaction","params":["120000228000000024000003FB61400000000000000168400000000000000C73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD0207446304402207268AD2FC68CD6AB8157F5BD0B0884F37DFF18D01E5414D6A277E33D4328D2BD02205502FDFFF00B7F1F9D7EB75329F65B50E619B6E1F9D24BE0D65F862419398A178114B5F762798A53D543A014CAF8B297CFF8F2F937E883145D2221763934C9380F43168B760199AC8C4DC074"],"id":1}' http://localhost:7545/v1/jsonrpc

// Result 结果
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": [
    "15392C8703C7141F3604C6DF251E5FB361FA3DF7B835843EF007EA51508FF77D"
  ],
  "status": "success"
}
```

***

#### jt_account

查询帐户历史交易

##### Parameters 参数

1. `STRING` 	- 所要查询余额的地址.
2. `INT` 		- 起始页号，可省略，默认为0
3. `INT` 		- 每页交易数, 可省略，默认为所有
4. `BOOL` 	- 是否最新的交易在前边，可省略，默认为true
5. `STRING`  - 交易方向，可省略，默认为'both',有'in', 'out'可选


```js
params: ["j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E", 0, 2, true, "in"]
```

##### Returns 返回值

`LIST` - 交易列表

##### Example 例子
```js
// Request 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"jt_account","params":["j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E", 0, 2, true, "in"],"id":1}' http://localhost:7545/v1/jsonrpc

// Result 结果
{
    "id": 1,
    "jsonrpc": "2.0",
    "result": [
        {
            "amount": "1",
            "date": "2018-09-27T12:49:00+08:00",
            "from_address": "jHb9CJAWyB4jr91VRWn96DkukG4bwdtyTh",
            "hash": "2EBDD99F0030C42127A7EB151F4852CC45F0CBADE7AB3A9B0321845C7EDCA826",
            "to_address": "j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E",
            "type": "Payment"
        },
        {
            "amount": "1000000000000",
            "date": "2018-09-27T12:30:40+08:00",
            "from_address": "jHb9CJAWyB4jr91VRWn96DkukG4bwdtyTh",
            "hash": "6E7751B22E630888C89AFF087D8A0197BA84704F19E7865611A2124C1CE8DE01",
            "to_address": "j9VSrHSiZPiJBPUS6iwYiT8yfy8iFbeR4E",
            "type": "Payment"
        }
    ],
    "status": "success"
}
```

***

## The default block parameter 默认区块参数

以下方法有额外的默认区块参数

- [jt_getBalance](#jt_getbalance)
- [jt_getTransactionCount](#jt_gettransactioncount)

当请求以太的状态时，最后一个默认区块参数决定区块的高度.

以下可能是默认区块参数:

- `Block Number String` - 区块编号，整数
- `Block Hash String` - 区块哈希，字符串
- `String "current"` - 表示当前区块
- `String "validated"` - 最新生效的区块
- `String "closed"` - 最近关闭的区块
