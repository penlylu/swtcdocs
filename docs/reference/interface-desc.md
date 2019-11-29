# 井通公链开发介绍

***

对接井通公链有以下几种方式：

* WebSocket

* Jingtum-lib

* Jingtum-api

* Swtc-lib

* Swtc-api

* Jcc-rpc


## WebSocket

***

Websocket获取jingtum账本的最基础的调用模式
Skywelld Websocket命令总结：
`http://bbswtc.com/forum.php?mod=viewthread&tid=216&extra=page%3D2`

## Jingtum-lib

***

Jingtum-lib是对websocket模式的接口进行的封装，封装成各个语言版本供开发者使用
Github代码仓库：
NodeJs版本：`https://github.com/swtcpro/jingtum-lib-nodejs`
Java版本：`https://github.com/swtcpro/jingtum-lib-java`
Python 版本：`https://github.com/swtcpro/jingtum-lib-python`
Go版本：`https://github.com/swtcpro/jingtum-lib-go`
C# 版本：`https://github.com/swtcpro/jingtum-lib-csharp`
Php 版本：`https://github.com/swtcpro/jingtum-lib-php`
C++ 版本：`https://github.com/swtcpro/jingtum-lib-cplusplus`

## Jingtum-lib

***

Jingtum-api是对Nodejs版本的jingtum-lib进行的又一次封装，对外提供api接口，使用者可以通过http请求直接获取接口数据。
Github代码仓库
NodeJs版本：`https://github.com/swtcpro/jingtum-api`
操作文档：`http://www.swtcdocs.org/zh_CN/latest/reference/jingtum-api/`

## Swtc-lib

***

Swtc-lib 是对jingtum-lib Nodejs版本的改造，此版本主要供钱包开发使用
NodeJs版本：`https://github.com/swtcpro/swtc-lib`

## Swtc-api

***

Swtc-api是对jingtum-api的二次封装。
NodeJs版本：https://github.com/swtcpro/swtc-api

## Jcc-rpc

***

Jcc-rpc是井畅提供的开源代码，其功能类似于Jingtum-lib，基于api做的封装，其接口功能主要应用于钱包的开发。
Nodejs版本：`https://github.com/JCCDex/jcc_rpc`
Java版本：`https://github.com/JCCDex/jcc_rpc_java`
Objective-C版本：`https://github.com/JCCDex/jcc_rpc_oc`
易语言版本：`https://github.com/JCCDex/jcc_rpc_yi`

## Service-rpc

***

Service-rpc是结合IPFS的一套应用接口，使用此接口需搭建ipfs节点。
操作文档：`http://www.swtcdocs.org/zh_CN/latest/reference/servicenode/#service`
