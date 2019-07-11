#1 CXCChain简介

CXC公链为多国极客团体联合开发，为整合了跨链技术、poA挖矿、原子交易、链商生态、通货紧缩经济模型、去中心化交易所+云盘+社交的次世代公链。

# 2.JSON-RPC API指令集

## 2.1 JSON-RPC协议概述

```
	JSON-RPC是基于json的跨语言远程调用协议。比xml-rpc、webservice等基于文本的协议数据传输格小；相对hessian、java-rpc等二进制协议便于调试、实现、扩展，是很优秀的一种远程调用协议。眼下主流语言都已有json-rpc的实现框架。
```

1.发起远程调用时向区块链节点传输格式例如以下:

```json
	{ "method": "showinfo", "configs": [], "id": 1}
```
> 参数说明：

```
	 method:调用的方法名
	 configs:方法传入的参数。数组类型。
	 id:调用标识符。用于标示一次远程调用过程
```
2.区块链节点其收到调用请求，处理方法调用，将方法效用结果效应给调用方；返回数据格式:

```json
	{"result":	{"paytxfee":0,"relayfee":0.1,"rpcport":7318,"maxout":2000000},"error":null,"id":"1"}
```
> 参数说明：

```
	result:方法返回值。
	error:调用时错误，无错误返回null。
	id:调用标识符，与调用方传入的标识符一致。
```
## 2.2 命令执行方式

RPC用户名密码存储在~/.cxcs/CXCChain/cxcs.conf（linux/MAC）或%APPDATA%\CXcs\CXCChain\cxcs.conf（Windows）文件中,可以使用cxcsi命令行工具或者CXCChain GUI工具(PC端钱包)内置的CLI界面连接，这些工具会自动读取RPC用户名密码并连接至已运行区块链,请不要将rpc port开放到外网。

> 命令方式运行：

```bash
	[command] [configs]
```
## 2.3 可用API指令

### 2.3.1区块链查询与控制

**调用命令的方式 cxcsi CXCChain (command),例如cxcsi CXCChain help.

#### help
> 方法说明

返回可用命令列表

> 调用命令

```bash
	help (command)
```
> 方法参数

|	参数|描述	|
|--	|--	|
|	command|命令名称或无参	|

> e.g.

```bash
	help pause
```

> 返回值

若无参数显示命令列表，若有参数显示该命令的使用方法

#### stop
> 方法说明

停止区块链运行

> 调用命令

```bash
	stop
```
> 方法参数

无

> 返回值

CXcs server stopping

#### pause
> 方法说明

暂停区块链

> 调用命令

```bash
	pause (task(s))
```
> 方法参数

|参数	|描述												|
|--		|--													|
|task(s)|要停止的服务，可能的值为incoming,mining,outchain	|
> e.g.

```bash
	pause incoming,mining
```

> 返回值

无

#### resume
> 方法说明

恢复区块链，与pause配合使用

> 调用命令

```bash
	resume (task(s))
```
> 方法参数

|参数	|描述												|
|--		|--													|
|task(s)|要重启的服务，可能的值为incoming,mining,outchain	|
> e.g.

```bash
	resume incoming,mining
```

> 返回值

无
#### showmem
> 方法说明

返回内存池信息

> 调用命令

```bash
	showmem
```
> 方法参数

无

> 返回值

```bash
	{
		"size" : 0,
		"bytes" : 0
	}
```
#### addnode
> 方法说明

手动添加或删除点对点连接

> 调用命令

```bash
	addnode "node" "add"|"remove"|"onetry"
```
> 方法参数
|参数	|描述	|
|--	|--	|
|node	|节点	|
|command	|add将节点加入连接集合;remove将节点删除出集合;onetry尝试连接节点测试	|
> e.g.

```bash
	addnode "ip:port" "add"
```
#### shownode
> 方法说明

查询连接节点

> 调用命令

```bash
	shownode  ( "node" )
```
> 方法参数
|参数	|描述	|
|--	|--	|
|(boolean)true/false|true=返回关于使用addnode添加节点的信息，false=返回使用addnode添加节点的列表	|
|node	|包含或省略ip(:port)的节点	|

> e.g.

```bash
	shownode true
	shownode true "ip"
```

> 返回值

```bash
	[
		{
			"addednode": "10.211.55.8:7319", 
			"connected": true, 
			"addresses": 
				[
					{
						"address": "10.211.55.8:7319", 
						"connected": "outbound"
					}
				]
		}
	]
```
#### shownet
> 方法说明

返回连接的网络IP及端口信息

> 调用命令

```bash
	shownet
```
> 方法参数

无


> 返回值

```bash
	{
  "version": xxxxx,                        The server version
  "subversion": "/Cxcs:x.x.x/",           The server subversion string
  "ProVer": xxxxx,                         The proBBBol version
  "localservices": "xxxxxxxxxxxxxxxx",     The services we offer to the network
  "timeoffset": xxxxx,                     The time offset
  "connections": xxxxx,                    The number of connections
  "networks": [                            Info per network
  {
    "name": "xxx",                         Network (ipv4, ipv6 or onion)
    "limited": true|false,                 If the network limited using -onlynet?
    "reachable": true|false,               If the network reachable?
    "proxy": "host:port"                   The proxy that is used for this network, or empty if none
  }
  ,...
  ],
  "relayfee": x.xxxxxxxx,                  Min relay fee for non-free deals in smg/kb
  "localaddresses": [                      List of local addresses
  {
    "address": "xxxx",                     Network address
    "port": xxx,                           Network port
    "score": xxx                           Relative score
  }
  ,...
  ]
}
```
#### showpeer
> 方法说明

返回连接到的其他节点的信息

> 调用命令

```bash
	showpeer
```
> 方法参数

无

> 返回值

```bash
	[
	  {
		"id": n,                           Peer index
		"addr":"host:port",                Ip address and port of the peer
		"addrlocal":"ip:port",             Local address
		"services":"xxxxxxxxxxxxxxxx",     The services offered
		"lastsend": ttt,                   The time in seconds since epoch (Jan 1 1970 GMT) of the last send
		"lastrecv": ttt,                   The time in seconds since epoch of the last receive
		"bytessent": n,                    The total bytes sent
		"bytesrecv": n,                    The total bytes received
		"conntime": ttt,                   The connection time in seconds since epoch
		"pingtime": n,                     ping time
		"pingwait": n,                     ping wait
		"version": v,                      The peer version
		"subver": "/Cxcs:0.2.1/",         The string version
		"handlocal": n,               Address used by local node for handshake.
		"handshake": n,                    Address used by remote node for handshake.
		"inbound": true|false,             Inbound or Outbound (false)
		"startingheight": n,               The starting height (block) of the peer
		"banscore": n,                     The ban score
		"synced_headers": n,               The last header with peer
		"synced_blocks": n,                The last block with peer
		"inflight": [
		   n,                              The heights of blocks we're currently asking from this peer
		   ...
		]
	  }
	  ,...
	]
```
#### showchain
> 方法说明

显示链信息

> 调用命令

```bash
	showchain
```
> 方法参数

无

> 返回值

```bash
	{
		"blocks" : 2016,
		"headers" : 2016,
		"bestblockhash" : "000061d0db856cf824e6e20e0eeb4005ac968a145b0aa142619a0127d073bdc2"
	}
```
#### showblock
> 方法说明

查看指定hash|height的区块信息

> 调用命令

```bash
	showblock "hash"|height ( type )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|hash/height	|区块哈希或区块高度	|
|type	|0- encoded data, 1- json object, 2- with tx encoded data, 4- with tx json object	|



> e.g.

```bash
	showblock "105"
```

> 返回值

```bash
	Outputs (for type = 1):
	{
	  (string)  "hash" : "hash",             The block hash
	  (string)  "miner" : "miner",           The address of the miner
	  (numeric) "confirmations" : n,         The number of confirmations
	  (numeric) "size" : n,                  The block size
	  (numeric) "height" : n,                The block height or index
	  (numeric) "version" : n,               The block version
	  (string)  "merkleroot" : "xxxx",       The merkle root
				"tx" : [                     The TXIDs
	  (string)            "TXID"             The TXID
							,...
						],
	  (numeric) "time" : ttt,                The block time in seconds since epoch (Jan 1 1970 GMT)
	  (numeric) "nonce" : n,                 The nonce
	  (string)  "bits" : "1000ffff",         The bits
	  (numeric) "difficulty" : x.xxx,        The difficulty
	  (string)  "prevblockhash" : "hash",The hash of the previous block
	  (string)  "nextblockhash" : "hash" The hash of the next block
	}

	Outputs (for type=0):
	  (string)  "data"                       A hex-encoded data string for block 'hash'.
```
#### showblocks
> 方法说明

返回有关指定块的信息

> 调用命令

```bash
	showblocks block-set-identifier ( false|true )
```
> 方法参数
|参数	|描述	|
|--	|--	|
|block-set-identifier	|区块高度或区块哈希或区块哈希/高度的集合或时间范围{"starttime" : start-time         Start time."endtime" : end-time             End time.}	|
|false|true	|默认为false,若为true将显示更详细的信息	|


> e.g.

```bash
	showblocks "hash1,hash2"
	showblocks "hash"
```

> 返回值

```bash
	区块信息集合
```
#### showblockhash
> 方法说明

返回指定高度块的HASH值

> 调用命令

```bash
	showblockhash index
```
> 方法参数
|参数	|描述	|
|--	|--	|
|index	|区块高度	|


> e.g.

```bash
	showblockhash 20
```

> 返回值

```bash
	区块hash值
```
#### signmessage
> 方法说明

根据私钥对消息进行签名

> 调用命令

```bash
	signmessage "address|privkey" "message"
```
> 方法参数
|参数		|描述			|
|--			|--				|
|address|privkey	|地址或私钥，若为地址，则必须保证该地址的私钥也在节点上			|
|message	|要签名的消息	|


> e.g.

```bash
	signmessage "addr" "my message"
```

> 返回值

```bash
	签名后的字符串
```
#### checkmessage
> 方法说明

验证消息

> 调用命令

```bash
	checkmessage "publickey" "signature" "message"
```
> 方法参数

|参数	|描述	|
|--	|--	|
|publickey	|公钥	|
|signature	|签名后的字符串	|
|message	|签名前的消息	|


> 返回值

```bash
			true|false
```
### 4.3.2管理钱包地址密钥
#### addnewaddr
> 方法说明

创建一个新地址

> 调用命令

```bash
	addnewaddr
```
> 方法参数

无

> 返回值

```bash
	创建出的新地址
```
#### addmultiaddr
> 方法说明

创建多签地址并添加至节点

> 调用命令

```bash
	addmultiaddr nrequired keys
```
> 方法参数

|参数	|描述	|
|--	|--	|
|nrequired	|签名时需要签名地址的数量	|
|keys	|公钥集合	|


> e.g.

```bash
	addmultiaddr 2 "[\"HEXPUBKEYS\",\"HEXPUBKEYS\"]"
```

> 返回值

```bash
	多签地址
```
#### setupmulti
> 方法说明

创建多签地址但不加入钱包，不建议使用

> 调用命令

```bash
	setupmulti nrequired keys
```
> 方法参数

|参数	|描述	|
|--	|--	|
|nrequired	|签名时需要签名地址的数量	|
|keys	|公钥集合	|

> e.g.

```bash
	setupmulti 2 "[\"HEX-PUBKEYS\",\"HEX-PUBKEYS\"]"
```

> 返回值

```bash
	{
	  "address":"multisigaddress",          The value of the new multisig address.
	  "redeemScript":"script"               The string value of the hex-encoded redemption script.
	}
```
#### setupkeypairs
> 方法说明

生成一个或多个公钥/私钥对，默认1

> 调用命令

```bash
	setupkeypairs ( count )
```
> 方法参数

|	参数|描述	|
|--	|--	|
|	count|创建公钥/私钥对的数量，默认为1	|


> e.g.

```bash
	setupkeypairs 10
```

> 返回值

```bash
	[
	   {
		  "address" : "address",            CXC address
		  "pubkey"  : "public key",         Public key
		  "privkey" : "private key",        Private key
	  }
	]
```
#### showaddrs
> 方法说明

返回此节点钱包中的地址详细信息

> 调用命令

```bash
	showaddrs ( addr(es) count start )
```
> 方法参数
|方法	|参数	|
|--	|--	|
|addr(es)	|*（显示所有地址）或addr(单个地址)或地址数组，默认为*	|
|count	|显示地址的数量,默认为INT_MAX	|
|	start|从第几条开始，默认为-count	|


> e.g.

```bash
	showaddrs
	showaddrs "*"
	showaddrs "ADDR"
```

> 返回值

```bash
	地址对象的数组
```
#### dumpprivkey
> 方法说明

导出私钥

> 调用命令

```bash
	dumpprivkey "address"
```
> 方法参数

|参数	|描述	|
|--	|--	|
|address	|要导出私钥的地址	|


> 返回值

```bash
	私钥
```
#### importprivkey
> 方法说明

导入私钥

> 调用命令

```bash
	importprivkey privkey(s)
```
> 方法参数

|参数		|描述									|
|--			|--										|
|privkey(s)	|必填，单个私钥或私钥数组，				|
|rescan		|是否重新扫描该钱包的交易，默认为true	|



> e.g.

```bash
	importprivkey "PRIKEY"
```

> 返回值

```bash
	无
```
#### importaddr
> 方法说明

将address 添加到钱包中(无私钥),创建一个或多个只读账户

> 调用命令

```bash
	importaddr address(es)
```
> 方法参数

|参数		|描述									|
|--			|--										|
|address(es)|必填，单个地址或地址数组				|
|rescan		|是否重新扫描该钱包的交易，默认为true	|


> e.g.

```bash
	importaddr "ADDR"
```

> 返回值

```bash
	无
```
#### backupwallet
> 方法说明

备份wallet.dat钱包文件

> 调用命令

```bash
	backupwallet "destination"
```
> 方法参数

|参数	|描述	|
|--	|--	|
|destination	|文件路径	|


> e.g.

```bash
	backupwallet "backup.dat"
```

> 返回值

```bash
	无
```
#### dumpwallet
> 方法说明

将钱包中的全部私钥转储为文本文件

> 调用命令

```bash
	dumpwallet "filename"
```
> 方法参数

|参数	|描述	|
|--	|--	|
|	filename|导出到的文件路径	|


> e.g.

```bash
	dumpwallet "test"
```

> 返回值

```bash
	无
```
#### encryptwallet
> 方法说明

首次加密节点的钱包， 使用password作为解锁密码
请注意，加密之后无法还原到无密码状态，除非删除节点所有文件，重新同步区块！

> 调用命令

```bash
	encryptwallet "password"
```
> 方法参数

|	参数|描述	|
|--	|--	|
|	password|密码	|


> e.g.

```bash
	encryptwallet "password"
```

> 返回值

```bash
	无
```
#### changepass
> 方法说明

更改钱包密码

> 调用命令

```bash
	changepass password newpassword
```
> 方法参数

|	参数|描述	|
|--	|--	|
|	password|旧密码	|
|	newpassword|新密码	|


> 返回值

```bash
	无
```
#### walletpass
> 方法说明

使用password解锁节点的钱包, 在time时间内有效。

> 调用命令

```bash
	walletpass password time
```
> 方法参数

|参数	|描述	|
|--	|--	|
|password	|密码	|
|	time|解锁时间，单位为秒	|


> e.g.

```bash
	walletpass password 60
```

> 返回值

```bash
	true|false
```
#### showassets
> 方法说明

返回链中已发行的资产的信息

> 调用命令

```bash
	showassets ( asset-id(s) detail count start )
```
> 方法参数

|参数		|描述														|
|--			|--															|
|asset-id(s)|资产名称或资产id或资产名称集合或资产id集合					|
|detail		|true or false,默认为false,若为true，则显示资产更详细的信息	|
|count		|显示数量,默认为INT_MAX										|
|start		|默认为-count												|


> e.g.

```bash
	showassets
	showassets "AAA"
```

> 返回值

```bash
	一个资产集合
```
#### showaddrbals
> 方法说明

返回此账户节点钱包中所有资产余额的列表

> 调用命令

```bash
	showaddrbals "address" ( minconf false|true )
```
> 方法参数

|参数		|描述									|
|--			|--										|
|address	|查询地址								|
|minconf	|最小区块确认值							|
|false/true	|默认为false,若为true，则包含只读账户	|


> e.g.

```bash
	showaddrbals "ADDR"
	showaddrbals "ADDR" 0 true
```

> 返回值

```bash
	每个地址余额
```
#### showallbals
> 方法说明

返回addresses指定的此节点钱包中的余额列表(包含原生货币)

> 调用命令

```bash
	showallbals ( address(es) assets minconf false|true false|true )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|address(es)	|查询余额的地址或地址集合	|
|assets	|查询的资产或资产集合	|
|minconf	|区块最小确认值	|
|false/true	|默认为false,若为true则包含只读账户	|
|false/true	|默认为false,若为true则包含锁定资产	|


> e.g.

```bash
	showallbals
	showallbals "address"
	showallbals "address" "AAA"
```

> 返回值

```bash
	返回余额
	
	{
    "1Mb8cBGeK5RUg7ezZ2KACt5PANu4fAxVjf" : [
        {
            "assetref" : "",
            "qty" : "100.00",
            "raw" : 100000000
        }
    ],
    "total" : [
        {
            "assetref" : "",
            "qty" : "100.00",
            "raw" : 100000000
        }
    ]
}
```
#### showaddrdeal
> 方法说明

返回此账户地址的交易记录

> 调用命令

```bash
	showaddrdeal "address" "txid"
```
> 方法参数

|参数	|描述		|
|--		|--			|
|address|地址		|
|txid	|The TXID	|


> e.g.

```bash
	showaddrdeal "address" "txid"
```

> 返回值

```bash
	[
  {
    "balance": {...},                  Changes in address balance.
    {
      "amount": x.xxx,                 The amount in native currency. Negative value means amount was send by the wallet, positive - received
      "assets": {...},                 Changes in asset amounts.
    }
    "myaddresses": [...],              Address passed as parameter
    "addresses": [...],                Array of counterparty addresses  involved in deal
    "permissions": [...],              Changes in permissions
    "sell": {...},                     Sell-details
    "data" : "metadata",               Hexadecimal representation of metadata appended to the deal
    "confirmations": n,                The number of confirmations for the deal.
    "blockhash": "hashvalue",          The block hash containing the deal.
    "blockindex": n,                   The block index containing the deal.
    "txid": "TXID",                    The TXID.
    "time": xxx,                       The deal time in seconds since epoch (midnight Jan 1 1970 GMT).
    "timereceived": xxx,               The time received in seconds since epoch (midnight Jan 1 1970 GMT).
    "comment": "...",                  If a comment is associated with the deal.
    "vin": [...],                      Array of input details
    "vout": [...],                     Array of output details
    "hex" : "data"                     Raw data for deal
  }
]
		
	
```
#### showaddrdeals
> 方法说明

返回此账户地址的交易信息

> 调用命令

```bash
	showaddrdeals "address" ( count skip false|true )
```
> 方法参数

|参数		|描述										|
|--			|--											|
|address	|地址										|
|count		|默认为10,显示记录的数量					|
|skip		|默认为0,记录查询的开始下标，				|
|false/true	|默认为false,若为true，则显示更加详细的信息	|


> e.g.

```bash
	showaddrdeals "ADDR"
	showaddrdeals "ADDR" 20 100
```

> 返回值

```bash
	[
	  {
	(object)  "balance": {...},                   Changes in address balance.
			{
	(numeric) "amount": x.xxx,                    The amount in native currency.
	(object)  "assets": {...},                    Changes in asset amounts.
			}
	(array)   "myaddresses": [...],               Address passed.
	(array)   "addresses": [...],                 Array of counterparty addresses  involved in deal
	(array)   "permissions": [...],               Changes in permissions
	(object)  "sell": {...},                      Sell details
	(array)   "data" : "metadata",                Hex metadata added to the deal
	 (numeric)"confirmations": n,                 The number of confirmations for the deal.
	(string)  "blockhash": "hashvalue",           The block hash containing the deal.
	(numeric) "blockindex": n,                    The block index containing the deal.
	(string)  "txid": "TXID",                     The TXID.
	(numeric) "time": xxx,                        The deal time.
	(numeric) "timereceived": xxx,                The time received.
	(string) "comment": "...",                    Comment with the deals.
	(array)  "vin": [...],                        If true. Array of input details
	(array)  "vout": [...],                       If true. Array of output details
	(string) "hex" : "data"                     If true. Raw data for deals
	  }
	]
```
#### showwalletdeal
> 方法说明

提供有关txid此节点钱包中的交易的信息

> 调用命令

```bash
	showwalletdeal "txid" ( false|true false|true )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|txid	|txid	|
|false/true	|默认为false,若为true,包含只读账户	|
|false/true	|默认为false,若为true,包含更加详细的交易记录信息	|


> e.g.

```bash
	showwalletdeal "TXID"
	showwalletdeal "TXID" true
```

> 返回值

```bash
	[
	  {
		"balance": {...},                 Changes in wallet balance.
		{
		  "amount": x.xxx,                The amount in native currency. Negative value means amount was send by the wallet, positive - received
		  "assets": {...},                Changes in asset amounts.
		}
		"myaddresses": [...],             Array of wallet addresses involved in deal
		"addresses": [...],               Array of counterparty addresses involved in deal
		"permissions": [...],             Changes in permissions
		"sell": {...},                    Sell-details
		"data" : "metadata",              Hexadecimal representation of metadata appended to the deal
		"confirmations": n,               The number of confirmations for the deal.
		"blockhash": "hashvalue",         The block hash containing the deal.
		"blockindex": n,                  The block index containing the deal.
		"txid": "TXID",                   The TXID.
		"time": xxx,                      The deal time in seconds since epoch (midnight Jan 1 1970 GMT).
		"timereceived": xxx,              The time received in seconds since epoch (midnight Jan 1 1970 GMT).
		"comment": "...",                 If a comment is associated with the deal.
		"vin": [...],                     If true. Array of input details
		"vout": [...],                    If true. Array of output details
		"hex" : "data"                    If true. Raw data for deal
	  }
	]
```
#### showunspent

> 方法说明

查询地址未花费输出

> 调用命令

```bash
	showunspent ( minconf maxconf addresses )
```
> 方法参数

|参数		|描述								|
|--			|--									|
|minconf		|区块最小确认数						|
|maxconf		|区块最大确认数				|
|addresses	|要查询的地址集合	|


> e.g.

```bash
	showunspent 6 9999999 "[\"address1\",\"address2\"]"
```

> 返回值

```bash
	[                                         Array of json object
    {
      "txid" : "txid",                      The TXID
      "vout" : n,                           The vout value
      "address" : "address",                The address
      "account" : "account",                The associated "" for the default account
      "scriptPubKey" : "key",               The script key
      "amount" : x.xxx,                     The deal amount in yqf
      "confirmations" : n                   The number of confirms
    }
    ,...
  ]
```

#### showwalletdeals

> 方法说明

查询有关钱包的交易记录

> 调用命令

```bash
	showwalletdeals ( count skip false|true)
```
> 方法参数

|参数		|描述								|
|--			|--									|
|count		|交易记录的数量						|
|skip		|开始的下表，默认为0				|
|false/true	|默认为false,若为true则包含只读账户	|


> e.g.

```bash
	showwalletdeals
	showwalletdeals 20 100 true
```

> 返回值

```bash
	[
	  {
		"balance": {...},                 Changes in wallet balance.
		{
		  "amount": x.xxx,                The amount in native currency.
		  "assets": {...},                Changes in asset amounts.
		}
		"myaddresses": [...],             Array of wallet addresses involved in deal
		"addresses": [...],               Array of counterparty addresses  involved in deal
		"permissions": [...],             Changes in permissions
		"sell": {...},                    Sell-details
		"data" : "metadata",              Hexadecimal representation of metadata appended to the deal
		"confirmations": n,               The number of confirms for the deal.
		"blockhash": "hashvalue",         The deal block hash.
		"blockindex": n,                  The deal block index.
		"txid": "TXID",                   The TXID.
		"time": xxx,                      The deal time in seconds since epoch (midnight Jan 1 1970 GMT).
		"timereceived": xxx,              The time received in seconds since epoch (midnight Jan 1 1970 GMT).
		"comment": "...",                 If a comment is associated with the deal.
		"vin": [...],                     Array of input details
		"vout": [...],                    Array of output details
		"hex" : "data"                    Raw data for deal
	  }
	]
```
#### showassetdeal
> 方法说明

根据资产id与txid查询交易详情

> 调用命令

```bash
	showassetdeal "asset-id" "txid" ( false|true )
```
> 方法参数

|参数		|描述									|
|--			|--										|
|asset-id	|资产id或资产名称						|
|txid		|txid									|
|false/true	|默认为false,若为true显示更加详细的信息	|


> e.g.

```bash
	showassetdeal "myasset" "mytxid"
	showassetdeal "myasset" "mytxid"  true
```

> 返回值

```bash
	交易详情
	
	"deal"                              Info about an individual deal from the perspective of a particular asset.
```
#### showassetdeals
> 方法说明

查询特定资产的交易信息

> 调用命令

```bash
	showassetdeals "asset-id" ( count start local-ordering )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|asset-id	|资产id或资产名称	|
|count	|显示数量	，默认为10|
|start	|开始条数，默认为-count	|
|local-ordering	|默认为false.false表示从钱包查询，true表示从链上查询	|


> e.g.

```bash
	showassetdeals "test-asset"
	showassetdeals "test-asset"  10 100 true
```

> 返回值

```bash
	List of deals
```
#### send
> 方法说明

发送一个或多个资产

> 调用命令

```bash
	send "address" object ( "comment" "comment-to" )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|address	|转账至的地址	|
|object	|若直接填写数量，则为发送原生资产，或者填写{"asset-name" : asset-count,"":   nativecurrency-count          Native currency use "",...}	|
|comment	|附加信息，不在区块链上	|
|comment-to	|附加信息归属，不在区块链上	|


> e.g.

```bash
	send "ADDRTO" "{\"assetname\":1000,\"\":2000}"
```

> 返回值

```bash
	txid
```
#### sendfrom
> 方法说明

指定地址发起转账

> 调用命令

```bash
	sendfrom "from-address" "to-address" quantities-object ( "comment" "comment-to" )
```
> 方法参数

|参数				|描述																																	|
|--					|--																																		|
|from-address		|发起地址																																|
|to-address			|目标地址																																|
|quantities-object	|若直接填写数量，则为发送原生资产，或者填写{"asset-name" : asset-count,"":   nativecurrency-count          Native currency use "",...}	|
|comment			|附加信息，不在区块链上																													|
|comment-to			|附加信息归属，不在区块链上																												|


> e.g.

```bash
	sendfrom "ADDRFROM" "ADDRTO" "{\"assetname\":1000,\"\":2000}"
	sendfrom "ADDRFROM" "ADDRTO" 0.1 "desc" "desc out"
```

> 返回值

```bash
	txid
```
#### sendasset
> 方法说明

发送一定数量的资产到指定地址

> 调用命令

```bash
	sendasset "address" "asset-id" count ( native-amount "comment" "comment-to" )
```
> 方法参数

|参数			|描述						|
|--				|--							|
|address		|地址						|
|asset-id		|资产id或资产名称			|
|count			|资产数量					|
|native-amount	|原生资产数量				|
|comment		|附加信息，不在区块链上		|
|comment-to		|附加信息归属，不在区块链上	|


> e.g.

```bash
	sendasset "ADDRTO" assetname 1000
	sendasset "ADDRTO" assetname 1000 0.1 "desc" "desc out"
```

> 返回值

```bash
	txid
```
#### sendassetfrom
> 方法说明

指定地址发起转账

> 调用命令

```bash
	sendassetfrom "from-address" "to-address" "asset-id" asset-count ( native-amount "comment" "comment-to" )
```
> 方法参数

|参数			|描述						|
|--				|--							|
|from-address	|发起转账地址				|
|to-address		|目的地址					|
|asset-id		|资产id或名称				|
|asset-count	|资产数量					|
|native-amount	|原生资产数量				|
|comment		|附加信息，不在区块链上		|
|comment-to		|附加信息归属，不在区块链上	|


> e.g.

```bash
	sendassetfrom "ADDRFROM" "ADDRTO" 0.1 assetname 1000
	sendassetfrom "ADDRFROM" "ADDRTO" 0.1 assetname 1000 "desc" "desc out"
```

> 返回值

```bash
	txid
```
#### senddata
> 方法说明

类似send， 附加数据

> 调用命令

```bash
	senddata "address" amount|asset-quantities data|send-new-item
```
> 方法参数

|参数				|描述														|
|--					|--															|
|address			|目的地址													|
|asset-quantities	|原生资产数量或资产obj										|
|data				|数据 hex,text,json,可以通过help with-data查看可传输的数据	|


> e.g.

```bash
	senddata "ADDR" "{\"11111-2222-5678\":1000,\"22222-3333-1234\":2000}" data-hex
	senddata "ADDR" 0.1 data-hex
```

> 返回值

```bash
	txid
```
#### senddatafrom
> 方法说明

指定地址发起转账并携带数据。

> 调用命令

```bash
	senddatafrom "from-address" "to-address" amount|asset-qty data|send-new-item
```
> 方法参数

|参数				|描述														|
|--					|--															|
|from-address		|发起地址													|
|to-address			|目的地址													|
|asset-quantities	|原生资产数量或资产obj										|
|data				|数据 hex,text,json,可以通过help with-data查看可传输的数据	|

> e.g.

```bash
	senddatafrom "ADDR" "ADDR" "{\"key\":100,\"key2\":200}" data-hex
	senddatafrom "ADDR" "ADDR" 0.1 data-hex
```

> 返回值

```bash
	txid
```
#### validaddr
> 方法说明

校验地址是否正确

> 调用命令

```bash
	validaddr "address"|"pubkey"|"privkey"
```
> 方法参数

|参数	|描述	|
|--	|--	|
|	|地址或公钥或私钥	|


> e.g.

```bash
	validaddr "ADDR"
```

> 返回值

```bash
	{
	  "isvalid" : true|false,           If the address is valid or not.
	  "address" : "address",            The address validated
	  "ismine" : true|false,            If the address is yours or not
	  "isscript" : true|false,          If the key is a script
	  "pubkey" : "publickeyhex",        The hex value of the raw public key
	  "iscompressed" : true|false,      If the address is compressed
	}
```
### 4.3.3创建发行资产/ChainDB

#### sell
> 方法说明

向地址发行一定数量的资产

> 调用命令

```bash
	sell "address" "asset-name"|asset-configs count ( unit native-amount extend-data )
```
> 方法参数

|参数			|描述				|
|--				|--					|
|address		|地址				|
|asset-configs	|资产名称或资产参数{"name":"asset1","open":true}，若open为true,则可增发			|
|count			|发行数量			|
|unit			|最小单位，默认为1	|
|native-amount	|原生资产数量		|
|extend-data	|附加数据			|


> e.g.

```bash
	sell "ADDR" "Asset1" 500
	sell "ADDR" "Asset1" 500 0.01
	sell "ADDR" '{"name":"asset1","open":true}' 500
```

> 返回值

```bash
	txid
```
#### sellfrom
> 方法说明

通过特定地址进行发行

> 调用命令

```bash
	sellfrom "from-address" "to-address" "asset-name"|asset-configs qty ( unit native-amount custom-fields )
```
> 方法参数

|参数			|描述																	|
|--				|--																		|
|from-address	|发起地址																|
|to-address"	|目标地址																|
|asset-configs	|资产名称或资产参数{"name":"asset1","open":true}，若open为true,则可增发	|
|count			|发行数量																|
|unit			|最小单位，默认为1														|
|native-amount	|原生资产数量															|
|extend-data	|附加数据																|

> e.g.

```bash
	sellfrom "ADDRFROM" "ADDRTO" "Asset1" true 1000
	sellfrom "ADDRFROM" "ADDRTO" Asset1  1000 0.01 0.01
```

> 返回值

```bash
	txid
```
#### sellasset
> 方法说明

向指定地址追加发行一定数量的资产

> 调用命令

```bash
	sellasset "address" "asset-id" qty ( native-amount custom-fields )
```
> 方法参数

|参数			|描述					|
|--				|--						|
|address		|目标地址				|
|asset-id		|资产id或资产名称		|
|qty			|追加数量				|
|native-amount	|原生资产数量			|
|custom-fields	|自定义参数，追加数据	|
> e.g.

```bash
	sellasset "ADDR" "Asset1" 1000
	sellasset "ADDR" Asset1 1000 0.01
```

> 返回值

```bash
	txid
```
#### sellassetfrom
> 方法说明

用特定地址向指定地址追加发行一定数量的资产

> 调用命令

```bash
	sellassetfrom "from-address" "to-address" "asset-id" qty ( native-amount custom-fields )
```
> 方法参数

|参数			|描述					|
|--				|--						|
|from-address	|发起地址				|
|to-address		|目标地址				|
|asset-id		|资产id或资产名称		|
|qty			|追加数量				|
|native-amount	|原生资产数量			|
|custom-fields	|自定义参数，追加数据	|

> e.g.

```bash
	sellassetfrom "Manage ADDR" "ADDRTO" "Asset1" 1000
	sellassetfrom "manage address" "ADDRTO" Asset1 1000 0.01
```

> 返回值

```bash
	txid
```
#### setupdatamod
> 方法说明

创建ChainDB，ChainDB以key-value的形式存储在区块链中

> 调用命令

```bash
	setupdatamod   "name" access  (extend-data )
```
> 方法参数

|参数		|描述						|
|--			|--							|
|name		|名称						|
|access		|true/false 是否任何人可写入|
|extend-data|附加数据					|


> e.g.

```bash
	setupdatamod datatest false
	setupdatamod datatest false '{"desc":"Test Data"}'
```

> 返回值

```bash
	txid
```
#### setupdatamodfrom
> 方法说明

用特定地址创建ChainDB

> 调用命令

```bash
	setupdatamodfrom "from-address"   "name" access  (extend-data )
```
> 方法参数

|参数			|描述						|
|--				|--							|
|from-address	|发起地址					|
|name			|名称						|
|access			|true/false 是否任何人可写入|
|extend-data	|附加数据					|

> e.g.

```bash
	setupdatamodfrom "ADDRFROM" dm1  false
	setupdatamodfrom "ADDRFROM" dm2  false '{"desc":"info"}'
```

> 返回值

```bash
	txid
```
#### senditem
> 方法说明

向ChainDB写入数据

> 调用命令

```bash
	senditem "datamodId" "key"|keys "data-hex"|data-obj "options"
```
> 方法参数

|参数		|描述																		|
|--			|--																			|
|datamodId	|DB名称或id															|
|"key"|/keys		|键值或键值数组																|
|data		|数据，包括hex,text,json等结构数据，可通过help with-data查看支持的数据格式	|
|options	|不填或outchain（脱链数据）													|

> e.g.

```bash
	senditem test "text" data
```

> 返回值

```bash
	txid
```
#### senditemfrom
> 方法说明

根据特定地址写入ChainDB

> 调用命令

```bash
	senditemfrom "from-address" "datamodid" "key"|keys "data-hex"|data-obj "options"
```
> 方法参数

|参数			|描述																		|
|--				|--																			|
|from-address	|发起地址																	|
|datamodId		|DB名称或id															|
|"key"/keys		|键值或键值数组																|
|data			|数据，包括hex,text,json等结构数据，可通过help with-data查看支持的数据格式	|
|options		|不填或outchain（脱链数据）													|

> e.g.

```bash
	senditemfrom "fromaddr" datamodname "key" hex
```

> 返回值

```bash
	txid
```
#### showdatas
> 方法说明

返回有关在区块链上创建的ChainDB的信息

> 调用命令

```bash
	showdatas ( DatamodId(s) false|true count start )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|DatamodId(s)	|DB名称或id或集合	|
|false/true	|默认为false,若为true将显示更加详细的信息	|
|count	|默认为INT_MAX，显示的数量	|
|start	|开始，默认为-count	|


> e.g.

```bash
	showdatas
```

> 返回值

```bash
	ChainDB集合
```
#### order
> 方法说明

订阅DB（只有订阅了才能DB查询的内容）

> 调用命令

```bash
	order entity-id(s) ( rescan )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|entity-id(s)	|DB id或DB名称	|
|rescan	|是否重新扫描钱包里面的交易，默认为true	|


> e.g.

```bash
	order "test-datamod" false
```

> 返回值

```bash
	无
```
#### noorder
> 方法说明

取消订阅DB

> 调用命令

```bash
	noorder entity-id(s) ( purge )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|entity-id(s)	|DB名称或集合	|
|purge	|true or false 默认为false,清除数据存储的所有外链数据	|


> e.g.

```bash
	noorder "datamod1"
```

> 返回值

```bash
	无
```
#### showdataitem
> 方法说明

检索Chain DB信息

> 调用命令

```bash
	showdataitem "datamod-id" "txid" ( detail )
```
> 方法参数

|参数|描述	|
|--	|--	|
|datamod-id	|Chain DB名称或Chain DB	|
|TXID	|TXID	|
|detail	|默认为false,若为true显示详细信息	|


> e.g.

```bash
	showdataitem "DATAMODID" "TXID"
	showdataitem "DATAMODID" "TXID"  true
```

> 返回值

```bash
	Chain DB数据信息
```
#### showdataitems
> 方法说明

查询ChainDB集合

> 调用命令

```bash
	showdataitems "datamodId" ( count start false|true )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|datamodIChainDB名称	|
|count	|显示数量，默认为10	|
|start	|	|
|false/true	|默认为false,若为true	|

> e.g.

```bash
	showdataitems "datamod1" 10 100
```

> 返回值

```basChainDB集合

```
#### showdatakeys
> 方法说明
ChainDB的关键字信息

> 调用命令

```bash
	showdatakeys "DatamodId" ( key(s) count start false|true )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|DatamodIChainDB名称	|
|key(s)	|默认为*，键值或键值集合，	|
|count	|默认为INT_MAX，显示数量	|
|start	|默认为-count	|
|false/true	|默认为false,true，钱包里面的数据，false，区块链里面的数据	|


> e.g.

```bash
	showdatakeys "datamod1"
	showdatakeys "datamod1" "key01"
	showdatakeys "datamod1" "*" 10 100
```

> 返回值

```bash
	key值数组
	
	[
		{
			"key":"key01",
			"items":1,
			"confirmed":1
		}
	]
```
#### showdatakeyitems
> 方法说明

根据键ChainDB

> 调用命令

```bash
	showdatakeyitems "datamodid" "key" (count start false|true )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|datamodiChainDB名称或id	|
|keChainDBkey值	|
|count	|默认为10，显示的条数	|
|start	|从后向前数的数量，默认为-count	|
|false/true	|若为false,在钱包内,若为true，在区块链内	|


> e.g.

```bash
	showdatakeyitems "datamod1" "key01"
	showdatakeyitems "datamod1" "key01" 10 100
```

> 返回值

```basChainDB数据集合
	Datamod item list
```
#### showdatasenderitems
> 方法说明

根ChainDB的地址进行检索

> 调用命令

```bash
	showdatasenderitems "datamodID" "address" ( false|true count start false|true )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|datamodIChainDB名称或id	|
|address	|地址	|
|false/true	|默认为false,若为true，显示更详细的信息	|
|count	|默认为10，显示的数量	|
|start	|默认为-count,若为负数从后面向前查询	|
|false/true	|默认为false,若为true在钱包内,若为false在区块中	|



> e.g.

```bash
	showdatasenderitems "datamod" "address"
	showdatasenderitems "datamod" "address" true 10 100
```

> 返回值

```basChainDB数据集合

```
#### showdatasenders
> 方法说明
ChainDB写入地址的信息

> 调用命令

```bash
	showdatasenders "datamodid" ( address(es) false|true count start false|true )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|datamodiChainDB名称或id	|
|address(es)	|默认为*,地址或地址集合	|
|false/true	|默认为false,若为true,显示发送者更详细的信息	|
|count	|默认为INT_MAX，显示数量	|
|start	|默认为-count,开始查询的条数，若为负数从后面向前查询	|
|false/true	|默认为false,false钱包中查询，true区块中	|


> e.g.

```bash
	showdatasenders "datamod1"
	showdatasenders "datamod1" address
	showdatasenders "datamod1" "*" true 10 100
```

> 返回值

```bash
	发送者数据列表
```
#### showtxoutdata
> 方法说明

查询指定交易的附加数据

> 调用命令

```bash
	showtxoutdata "txid" vout ( count-bytes start-byte )
```
> 方法参数

|参数		|描述						|
|--			|--							|
|txid		|txid						|
|vout		|vout						|
|count-bytes|默认为INT_MAX，查询字节数量|
|start-byte	|默认为0，开始字节数量		|


> e.g.

```bash
	showtxoutdata "TXID" 1
```

> 返回值

```bash
	附加数据信息，包括hex,text,json等结构的数据
```
### 原子交易

实现链上的原子交易所涉及到的命令。

#### prelock
> 方法说明

锁定未花费的输出用于原子交易。

> 调用命令

```bash
	prelock asset-qty ( true|false )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|asset-qty	|{"asset-id":qty}资产名称：数量	|
|true/false	|默认为true，若为true锁定资产，false不锁定	|


> e.g.

```bash
	prelock "{\"11111-2222-5678\":1000}"
	prelock "{\"11111-2222-5678\":1000,\"22222-3333-1234\":2000}"
```

> 返回值

```bash
	{
	  "txid": "TXID",                       TXID of the output which can be spent in setuprawex or setuprawex
	  "vout": n                             VOUT index
	}
```
#### prelockfrom
> 方法说明

指定来源地址进行资产锁定。

> 调用命令

```bash
	prelockfrom "from-address" asset-quantities ( lock )
```
> 方法参数

|参数				|描述										|
|--					|--											|
|from-address		|锁定地址									|
|asset-quantities	|{"asset-id":qty}资产名称：数量				|
|true/false			|默认为true，若为true锁定资产，false不锁定	|

> e.g.

```bash
	prelockfrom "ADDR" "{\"11111-2222-5678\":1000}"
	prelockfrom "ADDR" "{\"11111-2222-5678\":1000,\"22222-3333-1234\":2000}"
```

> 返回值

```bash
	{
	  "txid": "TXID",                            TXID of the output which can be spent in setuprawex or setuprawex
	  "vout": n                                  VOUT index
	}
```
#### setuprawex
> 方法说明

根据prelock操作产生的TXID和VOUT创建一个新的原子交换交易交换一定数量的资产,返回十六进制文本块TX-HEX，供对方交易

> 调用命令

```bash
	setuprawex "txid" vout ask-assets
```
> 方法参数

|参数	|描述	|
|--	|--	|
|txid	|prelock生成的txid	|
|vout	|prelock生成的vout	|
|ask-assets	|请求兑换的资产数量	|


> e.g.

```bash
	setuprawex TXID 1 "{\"ASSETID2\":100}"
```

> 返回值

```bash
	txid
```
#### decoderawex
> 方法说明

解码由setuprawex创建的tx-hex。

> 调用命令

```bash
	decoderawex "tx-hex" ( false|true )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|tx-hex	|要解析的hex	|
|false/true	|默认为false,若为true显示更加详细的信息	|


> e.g.

```bash
	decoderawex "HEX-STR"
```

> 返回值

```bash
	#兑换列表数据
	{
		"offer":{
			"amount":0,
			"assets":[
				{
					"name":"AAA",
					"assetref":"348-301-20955",
					"qty":100
				}
			]
		},
		"ask":{
			"amount":0,
			"assets":[
				{
					"name":"BBB",
					"assetref":"300-300-14411",
					"qty":100
				}
			]
		},
		"requiredfee":0.05,
		"candisable":true,
		"cancomplete":true,
		"complete":false
	}
```
#### addrawex
> 方法说明

接受完全报价/部分报价进行原子交易

> 调用命令

```bash
	addrawex "hex" "txid" vout ask-assets
```
> 方法参数

|参数	|描述	|
|--	|--	|
|hex	|setuprawex的返回值或addrawex的返回值	|
|txid	|prelock的返回值	|
|vout	|prelock的返回值	|
|ask-assets	|接受的资产及数量	|


> e.g.

```bash
	addrawex "HEX-STR" TXID 1 "{\"1234-5678-9090\":200}"
```

> 返回值

```bash
	{
	  "hex": "value",                    The raw deal with signature(s) (hex-encoded string)
	  "complete": true|false             If exchange is completed and can be sent
	}
```
#### completerawex
> 方法说明

完成一笔用于兑换的交易，如果有需要可附加手续费

> 调用命令

```bash
	completerawex hex txid vout ask-assets ( data|send-new-item )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|hex	|hex	|
|txid	|txid	|
|vout	|vout	|
|ask-assets	|接收资产及数量	|
|data/send-new-item	|携带的数据	|


> e.g.

```bash
	completerawex "hexstring" txid 1 "{\"1234-5678-1234\":200}"
```

> 返回值

```bash
	hex
```
#### sendrawdeal
> 方法说明

验证原子交易并进行广播交易。

> 调用命令

```bash
	sendrawdeal "tx-hex" ( allowhighfees )
```
> 方法参数

|参数			|描述								|
|--				|--									|
|tx-hex			|tx-hex								|
|allowhighfees	|默认为false,是否允许更高的手续费	|


> e.g.

```bash
	sendrawdeal "signedhex"
```

> 返回值

```bash
	txid
```
#### disrawdeal
> 方法说明

禁用交换的报价TX-HEX

> 调用命令

```bash
	disrawdeal "tx-hex"
```
> 方法参数

|参数	|描述	|
|--		|--		|
|tx-hex	|hex	|


> 返回值

```bash
	txid
```
#### gatherunspent
> 方法说明

归集未使用输出（UTXO）组合成一个未使用的输出，返回一个TXID列表，能够提升钱包的性能

> 调用命令

```bash
	gatherunspent ( "address(es)" minconf maxcombines mininputs maxinputs maxtime )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|address(es)	|默认为*,要归集的地址或地址集合	|
|minconf	|默认为1，最小确认数量	|
|maxcombines	|默认为100，最大归集的交易数量	|
|mininputs	|默认为2，最小输入数量	|
|maxinputs	|默认为100，最大输入数量	|
|maxtime	|默认为15s,最大广播时间	|


> e.g.

```bash
	gatherunspent "*" 1 15 2 10 100
	gatherunspent "addr"
```

> 返回值

```bash
	txids
```
#### showlock
> 方法说明

返回钱包中未使用的事务输出锁定列表

> 调用命令

```bash
	showlock
```
> 方法参数

无


> 返回值

```bash
	[
	  {
	   (string) "txid" : "TXID",                The TXID locked
	   (numeric) "vout" : n                     The vout value
	  }
	  ,...
	]
```
#### lock
> 方法说明

锁定或解锁未花费输出

> 调用命令

```bash
	lock unlock [{"txid":"txid","vout":n},...]
```
> 方法参数

|参数	|描述				|
|--		|--					|
|unlock	|false锁定，true解锁|
|deals	|要锁定或解锁的交易	|


> e.g.

```bash
	lock false "[{\"txid\":\"txid\",\"vout\":1}]"
	lock true "[{\"txid\":\"txid\",\"vout\":1}]"
```

> 返回值

```bash
	true or false,是否锁定或者解锁成功
```
#### setuprawdeal
> 方法说明

创建一个花费指定输入的事务，发送给给定的地址

> 调用命令

```bash
	setuprawdeal [{"txid":"id","vout":n},...] {"address":amount,...} ( [data] "action" )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|deals	|交易集合	|
|addresses	|地址：数量	|
|data	|附加数据	|
|action	|默认为“”,lock（锁定钱包中的给定输入），sign（使用钱包密钥签署交易），lock,sign，send（签署并发送交易	|



> e.g.

```bash
	setuprawdeal "[{\"txid\":\"myid\",\"vout\":0}]" "{\"address\":0.01}"
```

> 返回值

```bash
	#若action为""或lock或send
	hex
	#若action为其他几种
	{                                                        
	  "hex": "value",                                        The raw deal with signature(s) (hex-encoded string)
	  "complete": true|false                                 If deal has a complete set of signature (0 if not)
	}
```
#### setuprawsendfrom
> 方法说明

运用指定地址创建交易

> 调用命令

```bash
	setuprawsendfrom "from-address" {"address":amount,...} ( [data] "action" )
```
> 方法参数

|参数		|描述																										|
|--			|--																											|
|from-addr	|指定地址																									|
|addresses	|地址：数量																									|
|data		|附加数据																									|
|action		|默认为“”,lock（锁定钱包中的给定输入），sign（使用钱包密钥签署交易），lock,sign，send（签署并发送交易）	|

> e.g.

```bash
	setuprawsendfrom "FROMADDR" "{\"ADDR\":0.01}"
```

> 返回值

```bash
	同setuprawdeal
```
#### decoderawdeal
> 方法说明

解码交易的TX-HEX

> 调用命令

```bash
	decoderawdeal "tx-hex"
```
> 方法参数

|参数	|描述	|
|--	|--	|
|tx-hex	|要解析的hex	|


> 返回值

```bash
	{
	  "txid" : "id",                   The TXID
	  "version" : n,                   The version
	  "locktime" : ttt,                The lock time
	  "vin" : [
		 {
		   "txid": "id",               The TXID
		   "vout": n,                  The VOUT
		   "scriptSig": {              The script
			 "asm": "asm",             asm
			 "hex": "hex"              hex
		   },
		   "sequence": n               The script sequence number
		 }
		 ,...
	  ],
	  "vout" : [
		 {
		   "value" : x.xxx,            The value
		   "n" : n,                    Index
		   "scriptPubKey" : {
			 "asm" : "asm",            The asm
			 "hex" : "hex",            The hex
			 "reqSigs" : n,            The required signs
			 "type" : "pubkeyhash",    The type, eg 'pubkeyhash'
			 "addresses" : [
			   "address"  address
			   ,...
			 ]
		   }
		 }
		 ,...
	  ],
	}
```
#### addrawdeal
> 方法说明

类似setuprawdeal,将给定的输入和（常规或元数据）输出添加到指定的原始事务中TX-HEX，而不是创建新的事务

> 调用命令

```bash
	addrawdeal "tx-hex" [{"txid":"id","vout":n},...] ( {"address":amount,...} [data] "action" )
```
> 方法参数

|参数			|描述																									|
|--				|--																										|
|tx-hex			|创建交易后的hex																						|
|transactions	|交易集合																								|
|addresses		|资产：数量 可通过help addr-all进行查询																	|
|data			|附加数据																								|
|action			|默认为“”,lock（锁定钱包中的给定输入），sign（使用钱包密钥签署交易），lock,sign，send（签署并发送交易)	|

> e.g.

```bash
	addrawdeal "hexstring" "[{\"txid\":\"myid\",\"vout\":0}]" "{\"address\":0.01}"
```

> 返回值

```bash
	hex
```
#### addrawchange
> 方法说明

接受setuprawdeal 的输出。指定找零地址以及手续费

> 调用命令

```bash
	addrawchange "tx-hex" "address" ( fee )
```
> 方法参数

|参数	|描述		|
|--		|--			|
|tx-hex	|hex		|
|address|找零地址	|
|fee	|手续费		|

> e.g.

```bash
	addrawchange "HEX""ADDR"
	addrawchange "HEX""ADDR" 0.01
```

> 返回值

```bash
	hex
```
#### addrawdata
> 方法说明

为setuprawdeal的原始事务输出TX-HEX添加一个数据.

> 调用命令

```bash
	addrawdata tx-hex data
```
> 方法参数

|参数	|描述	|
|--	|--	|
|tx-hex	|hex	|
|data	|数据	|


> e.g.

```bash
	addrawdata "TX-HEX" data-hex
```

> 返回值

```bash
	hex
```
#### signrawdeal
> 方法说明

签署原始交易

> 调用命令

```bash
	signrawdeal "tx-hex" ( [{"txid":"id","vout":n,"scriptPubKey":"hex","redeemScript":"hex"},...] ["privatekey1",...] sighashtype )
```
> 方法参数

|参数	|描述	|
|--	|--	|
|tx-hex	|hex	|
|prevtxs	|交易数组	|
|privatekeys	|私钥数组	|
|sighashtype	|签署类型，默认为ALL|

> e.g.

```bash
	signrawdeal "HEX-STR"
```

> 返回值

```bash
	{
	  "hex": "value",                           The raw deal with signature(s) (hex-encoded string)
	  "complete": true|false                    If deal has a complete set of signature (0 if not)
	}
```

# 5.教程样例
## 5.1 常规交易样例
CXCChain采用UTXO 模型作底层存储的数据结构，全称为 Unspent Transaction output，也就是未被使用的交易输出。账户中的余额并不是由一个数字表示的，而是由当前区块链网络中所有跟当前账户有关的 UTXO 组成的。 CXCChain网络中具有两种类型资产，一种是本地货币，其运行在区块链网络的最底层，通过共识算法产生，没有符号，在底层标示为"",所有资产的转账交易都需要消耗原生货币，为了标识方面，命名为CXC;另一种为发行资产，是基于CXCChain公链的项目方发行的数字资产，通常具有相关符号表示。所有账号在转账交易或者进行事务处理时必须保证账户中有一定数量的CXC，否则无法转账成功。
### 5.1.1 创建多个新地址
```bash
	addnewaddr
	addr1
	addnewaddr
	addr2
	addnewaddr
	addr3
```
### 5.1.2 发送原生资产
```bash
	send addr1 10
	txid1
	sendfrom addr1 addr2
	txid2
```
### 5.1.3 发送非原生资产
```bash
	send addr1 '{"BBB":10}'
	txid1
	sendfrom addr1 addr2 '{"BBB":10}'
	txid2
```
### 5.1.4 发送多种资产
```bash
	send addr1 '{"BBB":10,"AAA":20,"":10}'
	txid1
	sendfrom addr1 addr2 '{"BBB":10,"AAA":20,"":10}'
	txid2
```
## 5.2 资产设计及创建资产样例
### 5.2.1 发行资产
* 找到一个举行发行数字资产权限的地址addr1
* 发行一个名为AAA的数字资产，创建10000个资产单位，设置最小单位为0.01,发行到addr2，并且可以追加发行
```bash
	sellfrom addr1 addr2 '{"name":"AAA","open":true}' 10000 0.01 
```
* 查看资产情况
```bash
	showassets AAA
	[
      {
          "name" : "AAA",
          "selltxid" : "XXXXXXXXXXXXXXXXXXXXXXXXXXXX",
          "assetref" : "XXXXX",
          "multiple" : 100,
          "units" : 0.01,
          "open" : true,
          "restrict" : {
              "send" : false,
              "receive" : false
          },
          "details" : {
          },
          "sellqty" : 10000,
          "sellraw" : 1000000,
          "ordered" : false
      }
  ]
```
### 5.2.2 追加发行资产
追加发行12000个AAA至addr2
```bash
	sellassetfrom addr1 addr2 AAA 12000 0.01
```
## 5.3 原子交换设计及创建资产样例

### 5.3.1 原子交易原理
任何CXC交易都可以有多个输入和输出，每个交易都可以涉及区块链上的不同地址。 CXC提供了许多API，可以轻松地执行原子交换。一方首先使用prelock (from)和setuprawex创建一个交换报价，在这个报价中指定他们提供的资产和数量，以及他们要求的资产和数量作为回报。此报价表示为部分交易，由发行方签署一个输入和一个输出。 然后可以将代表要约的部分交易发送给特定对方，或者分发给任何人审阅和接受。这种沟通可以在区块链上进行，例如使用发布到区块浏览器功能，或者使用任何所需的方法（甚至是电子邮件）进行脱链。另一位接收该邀请的参与者可以使用它进行审阅decoderawex，然后决定是否接受。如果是这样，他们prelock (from)和completerawex自己的输入和输出增加了交易，提供所要求的资产和数量，并要求提供资产和数量。然后最终的交易将被平衡，并可以使用广播到网络sendrawdeal。 CXC还提供disrawdeal API以在发布之后禁用报价。
### 5.3.2 原子交易流程
1.发行两个资产AAA、BBB。 

* 节点一执行,找到一个可以创建资产的地址:addr1,并创建资产发行至addr1
```bash
	sellfrom addr1 addr1 '{"name":"AAA","open":true}' 22000 0.01
```
* 节点二执行,找到一个可以创建资产的地址:addr2,并创建资产发行至addr2
```bash
	sellfrom addr2 addr2 BBB 5000 0.01
```
2.进行点对点原子交换
执行一个简单的原子交换，节点一1000 AAA交换节点二500 BBB。

* 节点二:创建包含500 BBB的锁定交易输出:
```bash
	prelockfrom addr2 '{"BBB":500, "":0.01}'
	
	{
      "txid" : "XXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
      "vout" : 0
	}
```
得到txid和vout
* 节点二:创建交易，指定交换1000 AAA
```bash
	setuprawex b6bb4dbdf088a0d1cdcb6db451c1bd9de80b59e449488301627f5c706ce3bf11 0 '{"AAA":1000}'
	
	HEX1
```
输出十六进制文本块，包含表示交换报价的原始交易数据。文本块记为HEX1
* 节点一:decoderawex HEX1 可查看兑换的内容
```bash
	decoderawex HEX1
  {
      "offer" : {
          "amount" : 0.01,
          "assets" : [
              {
                  "name" : "BBB",
                  "assetref" : "XXXXXXXXXXXXXXXX",
                  "qty" : 500
              }
          ]
      },
      "ask" : {
          "amount" : 0,
          "assets" : [
              {
                  "name" : "AAA",
                  "assetref" : "XXXXXXXXXXXXXXXXXXX",
                  "qty" : 1000
              }
          ]
      },
      "requiredfee" : 0.04,
      "candisable" : false,
      "cancomplete" : true,
      "complete" : false
  }
```
其中，cancomplete为true意味着此笔交易可以进行兑换。
* 节点一:创建1000 AAA的锁定交易输出
```bash
	prelockfrom addr1 '{"AAA":1000, "":0.02}'
  {
      "txid" : "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
      "vout" : 0
  }
```
得到txid和vout
* 节点一:添加到兑换交易，确认接受500 BBB交换
```bash
	addrawex HEX1 txid 0 '{"BBB":500}'

	{
        "hex" : HEX2,
        "complete" : true
    }
```
输出更长的十六进制文本，记为HEX2.
complete:true。这意味着交易完全签署并准备好进行广播和确认。
* 广播交易
```bash
	sendrawdeal HEX2
```
返回TXID.等待成块，查询余额可查询是否交换成功。
### 5.3.2 取消兑换
如何在接受和广播之前取消交换。
```bash
	disrawdeal HEX1
```
这时候再decoderawex 会返回error表示取消兑换成功。

## 5.4 多签账户交易及样例

### 5.4.1 多签账户简介

公钥加密也称为非对称密码学，是区块链的一个关键技术。链中的参与者生成自己的私钥和公共地址对。他们保密私钥，但是自由地分配相关的地址。对特定地址执行操作的区块链交易（例如花费其资金）必须由相应的私钥签名。然后链上的所有参与者都可以使用公共地址验证这些签名，而无需查看彼此的私钥。 共管合约账户也称多重签名，地址和交易通过在由多方共同管理的链上创建身份来拓宽这种模式。CXC使用“ m/n ” 其中多签地址，定义为:给定n个常规地址，至少对应于m个地址的私有密钥必须同时签署事务才能执行执行交易或事务。

### 5.4.2 创建共管合约账户地址

* 查找地址，找到地址addr1,addr2
* 创建共管账户 节点一运行以下命令来创建2/2的多重地址并将其添加到节点的钱包中:
```bash
	addmultiaddr 2 '["A1-publickey", "A2-publickey"]'
```
得到多签地址MA1A2 ，并自动开启跟踪 节点二可重复节点一命令
```bash
	addmultiaddr 2 '["A1-publickey", "A2-publickey"]'
```
### 5.4.3 通过多签账户交易

通过多签账户发送资金。因为这是一个2/2的共管合约账户，所以这个过程将需要两个地址的共同签名。
* 节点一:创建交易并部分签名
```bash
	setuprawsendfrom MA1A2 '{"XXXXXXXXXXXXXXXXXXXX ":{"AAA":500}}' '[]' sign
  {
      "hex" : "XXXXXXXXXXXXXXXXXXXXXXXX",
      "complete" : false
  }
```
响应complete:false，十六进制hex字段HEX1。已经部分签名。
* 节点二签名:
```bash
	  signrawdeal HEX1
  
	  {
		  "hex" : "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
		  "complete" : true
	  }
```
响应complete:true，十六进制hex字段HEX2。这意味着交易签名完成，可以向区块链广播。
* 广播交易
```bash
	sendrawdeal HEX2
```
## 5.5ChainDB

ChainDB为信息化系统和Dapp提供了完整解决方案，其侧重于一般数据检索，时间戳和存档，而不是参与者之间的资产转移。ChainDB理解为数据库的表或者是表中的字段。CXC目前支持单条事务处理量2M，单块16M(最高可达到64M)的性能，通过链外应用块的设计支持超大数据的分布式碎片化ChainDB支持集成化的链上链下混合存储。
### 5.5.1ChainDB
相当于传统信息系统开发的设计并实现表结构和字段 创建ChainDB，默认创建者ChainDB的全部操作许可，true代表任何人可写。 在节点一上
```bash
	setupdatamod DM1 true
```
### 5.5.ChainDB写数据
相当于传统信息系统开发的向数据库写数据 在节点一上
```bash
	#hex类型数据
	senditem DM1 key01 646174616d6f64
	#text文本数据
	senditem DM1 key02 '{"text":"Hello World"}'
	#json数据
	senditem DM1 key03 '{"json":{"name":"name1","age":20}}'
```
### 5.5.ChainDB检索数据
相当于传统信息系统开发的从数据库查询数据 在节点二上ChainDB:
```bash
	showdatas
    [
        {
            "name" : "XXXXXX",
            "setuptxid" : "XXXXXXXXXXXXXXXXXXXXXXXX",
            "datamodref" : "0-0-0",
            "restrict" : {
                "write" : false,
                "inchain" : false,
                "outchain" : false
            },
            "details" : {
            },
            "ordered" : true,
            "synchronized" : true,
            "items" : 0,
            "confirmed" : 0,
            "keys" : 0,
            "senditemers" : 0
        },
        {
            "name" : "DM1",
            "setuptxid" : "XXXXXXXXXXXXXXXXXXXXXXXX",
            "datamodref" : "XXXXXXXXXXXXXXXXXXXXXXXX",
            "restrict" : {
                "write" : false,
                "inchain" : false,
                "outchain" : false
            },
            "details" : {
            },
            "ordered" : false
        }
    ]
```
在数据存储上采用谁使用谁存储的原则，虽然全节点钱包存储全部区块，但不订阅则不结构化相关数据存储也不存储数据，订阅后才结构化相关数据及同步相关链外应用块及指针指向左旋链的链内应用块内容，避免浪费不必要的磁盘空间，节点二ChainDB的数据内容
```bash
	order DM1
	showdataitems DM1
	[
        {
            "senditemers" : [
                "XXXXXXXXXXXXXXXXXXXXXXXX"
            ],
            "keys" : [
                "key01"
            ],
            "outchain" : false,
            "available" : true,
            "data" : "646174616d6f64",
            "confirmations" : 16,
            "blockhash" : "XXXXXXXXXXXXXXXXXXXXXXXX",
            "blockindex" : 2,
            "blocktime" : 1533556931,
            "txid" : "XXXXXXXXXXXXXXXXXXXXXXXX",
            "vout" : 0,
            "valid" : true,
            "time" : 1533556931,
            "timereceived" : 1533557402
        },
        {
            "senditemers" : [
                "XXXXXXXXXXXXXXXXXXXXXXXX"
            ],
            "keys" : [
                "key02"
            ],
            "outchain" : false,
            "available" : true,
            "data" : {
                "text" : "hello world!"
            },
            "confirmations" : 15,
            "blockhash" : "XXXXXXXXXXXXXXXXXXXXXXXX",
            "blockindex" : 1,
            "blocktime" : 1533556951,
            "txid" : "XXXXXXXXXXXXXXXXXXXXXXXX",
            "vout" : 0,
            "valid" : true,
            "time" : 1533556951,
            "timereceived" : 1533557402
        }
    ]
```
有多种方式可以检索数据条目的内容，可ChainDB、通过数据条目发送者、通过关键字、通过时间戳等多种方式对数据进行查询，CXCChain在数据的存储上采用了结构化的数据存储方式，因此检索效率极高。
```bash
	# 显示ChainDB的关键索引
	showdatakeys DM1 
	# 显示key01为关键字的条目
    showdatakeyitems DM1 key01 
	# 显示ChainDB中的数据条目发布者信息
    showdatasenders DM1
	# 显示ChainDB中ADDRESS发布者发布的具体条目
    showdatasenderitems DM1 ADDRESS 
```
### 5.5ChainDB高级用法-JSON合并
```bash
	senditem DM1 key03 '{"json":{"name":"name1","city":"city1"}}'
    senditem DM1 key03 '{"json":{"city":"city2"}}'
	showdatakeysumm DM1 key03 jsonobjectmerge,ignoreother
    {
        "json" : {
            "city" : "city2",
            "name" : "name1"
        }
    }
	showdataitems DM1
    [
        {
            "senditemers" : [
                "XXXXXXXXXXXXXXXXXXXXXX"
            ],
            "keys" : [
                "key01"
            ],
            "outchain" : false,
            "available" : true,
            "data" : "646174616d6f64",
            "confirmations" : 37,
            "blockhash" : "XXXXXXXXXXXXXXXXXXXXXX",
            "blockindex" : 2,
            "blocktime" : 1533556931,
            "txid" : "XXXXXXXXXXXXXXXXXXXXXX",
            "vout" : 0,
            "valid" : true,
            "time" : 1533556930,
            "timereceived" : 1533556930
        },
        {
            "senditemers" : [
                "XXXXXXXXXXXXXXXXXXXXXX"
            ],
            "keys" : [
                "key02"
            ],
            "outchain" : false,
            "available" : true,
            "data" : {
                "text" : "hello world!"
            },
            "confirmations" : 36,
            "blockhash" : "XXXXXXXXXXXXXXXXXXXXXX",
            "blockindex" : 1,
            "blocktime" : 1533556951,
            "txid" : "XXXXXXXXXXXXXXXXXXXXXX",
            "vout" : 0,
            "valid" : true,
            "time" : 1533556943,
            "timereceived" : 1533556943
        },
        {
            "senditemers" : [
                "XXXXXXXXXXXXXXXXXXXXXX"
            ],
            "keys" : [
                "key03"
            ],
            "outchain" : false,
            "available" : true,
            "data" : {
                "json" : {
                    "name" : "Xiao Ming",
                    "city" : "Beijing"
                }
            },
            "confirmations" : 6,
            "blockhash" : "XXXXXXXXXXXXXXXXXXXXXX",
            "blockindex" : 1,
            "blocktime" : 1533557892,
            "txid" : "XXXXXXXXXXXXXXXXXXXXXX",
            "vout" : 0,
            "valid" : true,
            "time" : 1533557870,
            "timereceived" : 1533557870
        },
        {
            "senditemers" : [
                "XXXXXXXXXXXXXXXXXXXXXX"
            ],
            "keys" : [
                "key03"
            ],
            "outchain" : false,
            "available" : true,
            "data" : {
                "json" : {
                    "city" : "Shanghai"
                }
            },
            "confirmations" : 6,
            "blockhash" : "XXXXXXXXXXXXXXXXXXXXXX",
            "blockindex" : 2,
            "blocktime" : 1533557892,
            "txid" : "XXXXXXXXXXXXXXXXXXXXXX",
            "vout" : 0,
            "valid" : true,
            "time" : 1533557886,
            "timereceived" : 1533557886
        }
    ]
```
## 5.6 链外数据存储
* 节点一: 首先将超大数据（链外应用块）存储的数据写入缓存
```bash
	setupcache
```
得到缓存值记为cache1
* 将文件添加到缓存
```bash
	addcache cache1 file /Users/root/test.jpg
```
返回文件大小:956303
* 将缓存文件发布到区块链
```bash
	senditem DM1 key04 '{"cache":cache1}' outchain
```
返回TXID
* 节点二:查询数据
```bash
	showdatakeyitems DM1 key04
    [
        {
            "senditemers" : [
                "XXXXXXXXXXXXXXXXXXXXXX"
            ],
            "keys" : [
                "key04"
            ],
            "outchain" : true,
            "available" : true,
            "data" : {
                "txid" : "XXXXXXXXXXXXXXXXXXXXXX",
                "vout" : 0,
                "format" : "raw",
                "size" : 956303
            },
            "confirmations" : 2,
            "blockhash" : "XXXXXXXXXXXXXXXXXXXXXX",
            "blockindex" : 1,
            "blocktime" : 1533558603,
            "txid" : "XXXXXXXXXXXXXXXXXXXXXX",
            "vout" : 0,
            "valid" : true,
            "time" : 1533558597,
            "timereceived" : 1533558597
        }
    ]
```
* 可以看到列出的新项目。 如果数据"available" : false 说明链外应用块尚未同步完成，可以查询同步队列
```bash
	showappblockq
	
    {
        "appblocks" : {
            "waiting" : 0,
            "querying" : 0,
            "retrieving" : 0
        },
        "bytes" : {
            "waiting" : 0,
            "querying" : 0,
            "retrieving" : 0
        }
    }
```
* 一旦块队列为空，项目的数据应该可用，可以以十六进制查看其内容:
```bash
	showtxoutdata TXID 0 512
```
# 6.场景化智能合约参考

## 6.1 代币融资智能合约参考

主要目标:设置自动化的原子交易，实现代币融资智能合约的底层技术框架。 场景描述:创建一种待融资的资产（代币代码CCC，最小单位0.01，总量10000000，融资额5000000，兑换500 CXC（系统中已存在的资产或原生货币））
* 在节点服务器上创建一个新的地址
```bash
	#得到地址A2
	addnewaddr 
```
* 从管理地址A1向A2发行CCC 10000000个，最小单位0.01，不可再次追加。
```bash
	sellfrom A1 A2 '{"name":"CCC","open":false}' 10000000 0.01 0.01 '{"Build":"CN", "Index":"01", "for":"OfferCoin"}'
	#返回TXID
	#查看新资产发行情况
	showassets CCC
```
* 发起代币融资原子交易合约
```bash
	  #现在让我们创建一个新的交易。

	  prelockfrom A2 '{"CCC":5000000}'

	  #得到txid:TXID1
	  #得到vout:VOUT1

	  #建立原子交易，指定我们要用500 CXC（底层编码""）兑换5000000 CCC:

	  setuprawex TXID1 VOUT1 '{"":500}'

	  #输出十六进制块HEX1，然后将此HEX1通ChainDB自动发送给所有节点，也可通过线下发送给参与方。
```
* 参与者1参与兑换
```bash
	  prelockfrom 1-address '{"":100}'

	  #得到txid:TXID2
	  #得到vout:VOUT2

	  #现在我们将这个100 CXC的报价加到交易中，要求交换1000000CCC（按原始报价比例）:

	  addrawex HEX1 TXID2 VOUT2 '{"CCC":1000000}'

	  #输出十六进制HEX2， complete : false，这意味着交换尚未平衡。查看在交易中哪些资产仍然被提供和请求:

	  decoderawex HEX2

	  #这应该表明还有4000000 CCC的报价，还有400 CXC的要求。有关更详细的细目:

	  decoderawex HEX2 true

	  #该exchanges字段显示的所有个人offer和ask阶段的交换交易，到目前为止，旁边的address参与。
```
* 现在参与者2继续参与兑换:
```bash
	  prelockfrom 2-address '{"":200}'

	  #得到txid:TXID3
	  #得到vout:VOUT3

	  addrawex HEX2 TXID3 VOUT3 '{"CCC":2000000}' 

	  #输出最长的十六进制 HEX3
```
* 参与者3继续参与兑换
```bash
	  prelockfrom 3-address '{"":200}'

	  #得到txid:TXID4
	  #得到vout:VOUT4

	  completerawex HEX3 TXID4 VOUT4 '{"CCC":2000000}' 4322074fdf6872d65652077617973

	  #输出十六进制 HEX4，可以广播确认:

	  sendrawdeal HEX4

	  #至此，完成了整个代币融资过程。 
```

