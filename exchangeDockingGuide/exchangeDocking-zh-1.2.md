# 交易所对接指南

本文的目的主要是协助交易所上线CXC以及基于CXCChain所发行的代币。

github:https://github.com/cxcblock

## 基本概念

CXC的账户体系采用了UTXO模型，所以上线CXC是和BTC是比较类似的。

## Linux 节点部署

### 安装

```shell
 下载 https://github.com/cxcblock/cxcs/archive/master.zip
 解压 master.zip并进入
 为文件加执行权限 sudo chmod +x install.sh
 执行：./install.sh
```

### 启动

#### 启动正式链节点

` ./cxcsz CXCChain` 

### CLI交互方式管理节点

#### 管理正式链节点

` ./cxcsi CXCChain `

### 停止节点

#### 停止正式链节点

` ./cxcsi CXCChain stop `

### 启动时连接指定节点

```bash
./cxcsz CXCChain@ip:port
```

## mac 节点部署

### 安装

```shell
 下载 https://github.com/cxcblock/cxcs/archive/master.zip
 解压 master.zip并进入
 为文件加执行权限 sudo chmod +x install.sh
 ./install.sh
```

### 启动

#### 启动正式链节点

` ./cxcsz CXCChain` 

### CLI交互方式管理节点

#### 管理正式链节点

` ./cxcsi CXCChain `

### 停止节点

#### 停止正式链节点

` ./cxcsi CXCChain stop `

### 启动时连接指定节点

./cxcsz CXCChain@ip:port

## window 节点部署

### 安装

```shell
 下载 https://github.com/cxcblock/cxcs/archive/master.zip
 解压 master.zip并进入
 右键点击install.bat选择已管理员身份运行
```

### 启动

#### 启动正式链节点

` cxcsz.exe CXCChain` 

### CLI交互方式管理节点

#### 管理正式链节点

` cxcsi.exe CXCChain `

### 停止节点

#### 停止正式链节点

` cxcsi.exe CXCChain stop `

### 启动时连接指定节点

cxcsz.exe CXCChain@ip:port

启动后将开始同步区块，同时启动RPC服务（如何使用命令可查看开发者指南文档）。

## 节点启动重要参数

参数在cxcsz CXCChain 后增加 以-开头，后面跟参数名=参数值

存储目录，如果采用默认，请不要添加

```bash
-datadir=/data/.cxcs
```

区块链端口，如果采用默认，请不要添加

```bash
-port=7319
```

rpc命令端口，如果采用默认，请不要添加

```bash
-rpcport=7318
```

允许调用rpc的ip，如果不设定，请不要添加

```bash
-rpcallowip=127.0.0.1
```

每次生成新区块触发指定shell命令，其中%s是新区块的HASH,如果不需要，请不要添加

```bash
-blocknotify='curl http://192.168.1.10:8080/cxc?blockHash=%s'
```

例如:

```bash
./cxcsz CXCChain -datadir=/data/.cxcs -port=7319 -rpcport=7318 -blocknotify=‘curl http://192.168.1.10:8080/cxc?blockHash=%s'
```

链启动后可以将启动参数写到conf配置文件中，配置文件位于/data/.cxcs/CXCChain/cxcs.conf

例如：

```bash
rpcuser=cxcsrpc
rpcpassword=5ZH88wEUMDnTQm4bBitWcSKAyXjdMNC3aYAVP6BjhwGA
port=7319
rpcport=7318
rpcallowip=127.0.0.1
blocknotify='curl http://192.168.1.10:8080/cxc?blockHash=%s'
#添加多个allowip
rpcallowip=192.168.0.100
rpcallowip=192.168.0.101
rpcallowip=192.168.0.102
```

设置后需重启节点。

通过以下命令可查看节点基本信息

```bash
showinfo
```

返回结果为

```json
{
    "paytxfee" : "0.00",
    "rpcport" : 7318,
    "relayfee" : "0.0001",
    "maxout" : 10000000000000000
}
```

其中paytxfee为当前节点的基础手续费设置，rpcport为可调用的rpc端口

必须通过settxfee设置最小手续费，否则可能导致优先级不足！！

```bash
settxfee 0.0001
```

再调用showinfo查看信息

## 如何通过rpc调用命令

1. 确认已经将调用方IP配入allowip中

2. 查看区块链目录下cxcs.conf中账号密码.

   如果未指定区块链目录Linux、Mac默认目录为~/.cxcs/CXCChain 

   ​									Windows默认目录为C:\users\\{username}\AppData\Romaming\CXCChain

3. 向节点RPC发起HTTP请求

   如下：调用setupkeypairs命令创建两个新地址

   ```bash
    $ curl --user cxcsrpc:Ch6iaD7aPqegYDenzkDz3ttFCaBUTzBeqsmgye5Mn98o --data-binary '{"jsonrpc": "2.0", "id":"rpccall", "method": "setupkeypairs", "configs": [2] }' -H 'content-type: text/plain;' http://127.0.0.1:7318
    
    {"result":[{"address":"address1","pubkey":"pubkey1","privkey":"pribkey1"},{"address":"address2","pubkey":"pubkey2","privkey":"privkey2"}],"error":null,"id":"rpccall"}
   ```

## 生成充值地址

充值地址有如下两种生成方式：

- 方式一：动态创建CXCChain地址

  调用如下命令：

  ```bash
  ./cxcsi CXCChain addnewaddr
  ```

  后面将隐藏./cxcsi CXCChain.

  创建好的地址会直接导入到钱包文件中。

- 方式二：提前批量创建钱包地址

  调用如下命令：

  ```bash
  setupkeypairs (count)
  ```

  其中参数count为公钥/私钥对的数量，默认为1，注意这种方式创建的地址需要通过importaddr或者importprivkey的命令将地址导入钱包，才能直接调用转账的命令或者查询未花费输出。

  例如：

  - setupkeypairs 100 
  - importaddr '["ADDR1","ADDR2","ADDR3"]' false

  ## 检测地址的有效性

  通过调用如下命令检测账户地址的有效性：

  ```bash
  validaddr (address)
  ```

  返回结果如下：

  ```json
  {
    "isvalid" : true|false,           If the address is valid or not.
  "address" : "address",            The address validated
    "ismine" : true|false,            If the address is yours or not
    "isscript" : true|false,          If the key is a script
    "pubkey" : "publickeyhex",        The hex value of the raw public key
    "iscompressed" : true|false,      If the address is compressed
  }
  ```

若isvalid字段为false，则为无效地址。

## 监控用户充值

监控用户充值首先要查看区块同步情况，解析区块信息，从而确认用户是否进行充值。

### 查看区块同步情况

  调用命令如下：

```bash
  showchain
```

  返回结果如下：

```json
  {
    "blocks" : 13169,
      "headers" : 14009,
      "bestblockhash" : "00c44114d67728e2ea1ba1c323aa6e4d8ef18d8ee178a6d712634c775fc56614"
  }
```

  其中headers是区块头的数量，blocks是已同步的区块数量。

### 解析区块信息

  调用命令如下：

```bash
  showblock hash|height 4
```

  账户体系采用了UTXO模型，交易所需要解析每个TX中的VOUT部分，其中value为本地资产即CXC,assets为发行资产（注意，在进行发行资产转账时需要消耗CXC作为手续费）。有两种方式比对充值信息：

1. 交易所记录下所有和自己相关的交易，作为用户充值提现的记录。如果发现在VOUT中有属于交易所的地址，则修改数据库中该充值地址的用户余额（当然也可能包含发行资产）.
2. 如果在VOUT中有属于交易所的地址，先在数据库中记录下充值记录，等待几个区块确认后再修改用户余额，注意此处需要检查vin和vout中的重复地址，判断vout中是转账行为、归集行为还是找零行为。

```bash
  showblock 0007151d74d2330167dc6714092f63e864b7861467b06fa89659487ae88bc0b3 4
```

  返回结果：

```json
 {
    "hash" : "0007151d74d2330167dc6714092f63e864b7861467b06fa89659487ae88bc0b3",
    "miner" : "14T6N3jb5bdND3Hzb7M3qQJHYs9fDP9hC2",
    "confirmations" : 2,
    "size" : 621,
    "height" : 218364,
    "version" : 3,
    "merkleroot" : "6536cdac7472a9f08594557f5e720d1a94ff2f903821ec8afd3a3440deae5f9f",
    "tx" : [
        {
            "txid" : "4c6752c4088cd1dc0ad59737d0dbdae4108e29e64e8711f199b5edffbed40d6c",
            "version" : 1,
            "locktime" : 0,
            "vin" : [
                {
                    "coinbase" : "03fc54030101062f503253482f",
                    "sequence" : 4294967295
                }
            ],
            "vout" : [
                {
                    "value" : "125.0001",
                    "n" : 0,
                    "scriptPubKey" : {
                        "asm" : "OP_DUP OP_HASH160 25d7ab47ea9f31e88235cba5ca24457b54543933 OP_EQUALVERIFY OP_CHECKSIG",
                        "hex" : "76a91425d7ab47ea9f31e88235cba5ca24457b5454393388ac",
                        "reqSigs" : 1,
                        "type" : "pubkeyhash",
                        "addresses" : [
                            "14T6N3jb5bdND3Hzb7M3qQJHYs9fDP9hC2"
                        ]
                    }
                },
                {
                    "value" : "0.00",
                    "n" : 1,
                    "scriptPubKey" : {
                        "asm" : "OP_RETURN 53504b62473045022100b1c7371d57544ec1d700c21c6a11170ed25fd1689bf69096bde6de81502e3aa002207a4bc58eb23fd9ade2767481a7d1eeec925241b4fd81373ae11b44c4886411ba022102fbbaafc765ad3ec05d6b5bd9274cc64c393f0c11d5e4572141eb8c0aec2761ae",
                        "hex" : "6a4c6f53504b62473045022100b1c7371d57544ec1d700c21c6a11170ed25fd1689bf69096bde6de81502e3aa002207a4bc58eb23fd9ade2767481a7d1eeec925241b4fd81373ae11b44c4886411ba022102fbbaafc765ad3ec05d6b5bd9274cc64c393f0c11d5e4572141eb8c0aec2761ae",
                        "type" : "nulldata"
                    },
                    "data" : [
                        "53504b62473045022100b1c7371d57544ec1d700c21c6a11170ed25fd1689bf69096bde6de81502e3aa002207a4bc58eb23fd9ade2767481a7d1eeec925241b4fd81373ae11b44c4886411ba022102fbbaafc765ad3ec05d6b5bd9274cc64c393f0c11d5e4572141eb8c0aec2761ae"
                    ]
                }
            ],
            "hex" : "01000000010000000000000000000000000000000000000000000000000000000000000000ffffffff0d03fc54030101062f503253482fffffffff02a4597307000000001976a91425d7ab47ea9f31e88235cba5ca24457b5454393388ac0000000000000000726a4c6f53504b62473045022100b1c7371d57544ec1d700c21c6a11170ed25fd1689bf69096bde6de81502e3aa002207a4bc58eb23fd9ade2767481a7d1eeec925241b4fd81373ae11b44c4886411ba022102fbbaafc765ad3ec05d6b5bd9274cc64c393f0c11d5e4572141eb8c0aec2761ae00000000"
        },
        {
            "txid" : "f854d30f9679fb2f810ba3344d2e2614059e27f65be1220f02c82f76e4d61eef",
            "version" : 1,
            "locktime" : 0,
            "vin" : [
                {
                    "txid" : "e8d14543ea296aa3432e6fa5a39dcfc6324d8fb791e2d155f49770f1d5ed688c",
                    "vout" : 0,
                    "scriptSig" : {
                        "asm" : "30440220498672ef16691868fc36e0a56aeecc379b30ea9eeca086a3dbf627564490d45b02200a913b1747da3da56ca384a964a47e7e4ccdc7bc30e85892be9f38281e5e7d0e01 03016a820dbc2e1d0f3bf1c8a9b28a977b899fa57a74d005d83a5b25410988fb17",
                        "hex" : "4730440220498672ef16691868fc36e0a56aeecc379b30ea9eeca086a3dbf627564490d45b02200a913b1747da3da56ca384a964a47e7e4ccdc7bc30e85892be9f38281e5e7d0e012103016a820dbc2e1d0f3bf1c8a9b28a977b899fa57a74d005d83a5b25410988fb17"
                    },
                    "sequence" : 4294967295
                }
            ],
            "vout" : [
                {
                    "value" : "0.10",
                    "n" : 0,
                    "scriptPubKey" : {
                        "asm" : "OP_DUP OP_HASH160 a781c82edc6048a44e7e35ac0029d9447851fb39 OP_EQUALVERIFY OP_CHECKSIG 73706b71602cebc09e7d8999b42b95434272eca60a00000000000000 OP_DROP",
                        "hex" : "76a914a781c82edc6048a44e7e35ac0029d9447851fb3988ac1c73706b71602cebc09e7d8999b42b95434272eca60a0000000000000075",
                        "reqSigs" : 1,
                        "type" : "pubkeyhash",
                        "addresses" : [
                            "1GGhKSnxh1DgNB6gSB4RuJZaJ2vAjV1xrS"
                        ]
                    },
                    "assets" : [
                        {
                            "name" : "CoinA",
                            "selltxid" : "a6ec724243952bb499897d9ec0eb2c600ef977a7074ac6d862cfffebf1d9a400",
                            "assetref" : "225-301-60582",
                            "qty" : 0.1,
                            "raw" : 10,
                            "type" : "transfer"
                        }
                    ]
                },
                {
                    "value" : "0.00",
                    "n" : 1,
                    "scriptPubKey" : {
                        "asm" : "OP_DUP OP_HASH160 0d688d7bc67854dd1278e14a1ec52c47242c00da OP_EQUALVERIFY OP_CHECKSIG 73706b71602cebc09e7d8999b42b95434272eca6ea01000000000000 OP_DROP",
                        "hex" : "76a9140d688d7bc67854dd1278e14a1ec52c47242c00da88ac1c73706b71602cebc09e7d8999b42b95434272eca6ea0100000000000075",
                        "reqSigs" : 1,
                        "type" : "pubkeyhash",
                        "addresses" : [
                            "12Du2wZN7R9HqLxssq2xFhjPcjSj5vehCb"
                        ]
                    },
                    "assets" : [
                        {
                            "name" : "CoinA",
                            "selltxid" : "a6ec724243952bb499897d9ec0eb2c600ef977a7074ac6d862cfffebf1d9a400",
                            "assetref" : "225-301-60582",
                            "qty" : 4.9,
                            "raw" : 490,
                            "type" : "transfer"
                        }
                    ]
                },
                {
                    "value" : "0.8999",
                    "n" : 2,
                    "scriptPubKey" : {
                        "asm" : "OP_DUP OP_HASH160 0d688d7bc67854dd1278e14a1ec52c47242c00da OP_EQUALVERIFY OP_CHECKSIG",
                        "hex" : "76a9140d688d7bc67854dd1278e14a1ec52c47242c00da88ac",
                        "reqSigs" : 1,
                        "type" : "pubkeyhash",
                        "addresses" : [
                            "12Du2wZN7R9HqLxssq2xFhjPcjSj5vehCb"
                        ]
                    }
                }
            ],
            "hex" : "01000000018c68edd5f17097f455d1e291b78f4d32c6cf9da3a56f2e43a36a29ea4345d1e8000000006a4730440220498672ef16691868fc36e0a56aeecc379b30ea9eeca086a3dbf627564490d45b02200a913b1747da3da56ca384a964a47e7e4ccdc7bc30e85892be9f38281e5e7d0e012103016a820dbc2e1d0f3bf1c8a9b28a977b899fa57a74d005d83a5b25410988fb17ffffffff03a0860100000000003776a914a781c82edc6048a44e7e35ac0029d9447851fb3988ac1c73706b71602cebc09e7d8999b42b95434272eca60a000000000000007500000000000000003776a9140d688d7bc67854dd1278e14a1ec52c47242c00da88ac1c73706b71602cebc09e7d8999b42b95434272eca6ea01000000000000753cbb0d00000000001976a9140d688d7bc67854dd1278e14a1ec52c47242c00da88ac00000000"
        }
    ],
    "time" : 1570967695,
    "nonce" : 714,
    "bits" : "2000ffff",
    "difficulty" : 5.96046447753906e-8,
    "chainwork" : "000000000000000000000000000000000000000000000000000000000354fd00",
    "prevblockhash" : "00ba503c3f671a5c9593b0c37d1bb1b15987e87f171a8d6b315dbb20f7ac26b5",
    "nextblockhash" : "0033a95d00c16c344549201c7cc25d7a8594b18a745fb56dedcb5823fd3b2c09"
}
```

## 处理提现请求


1. 记录用户提现请求，更改余额；
2. 调用send或者sendfrom命令，向用户提现地址发送交易(CXC以及链上发行资产均可)，具体使用方法请参考《[开发者文档](https://github.com/cxcblock/doc/tree/master/developer)》；
3. 调用命令成功后将返回txid，记录在数据库中；
4. 等待区块确认后，即为提现成功。

注意，通过send或者sendfrom命令进行转账时，默认会找零会发起转账的地址，如需指定找零地址，请采用离线交易的方式。

## 离线交易

### 查询未花费输出

调用如下命令：

```bash
showunspent ( minconf maxconf addresses )
```

> 方法参数

| 参数      | 描述             |
| --------- | ---------------- |
| minconf   | 区块最小确认数   |
| maxconf   | 区块最大确认数   |
| addresses | 要查询的地址集合 |

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

### 组装交易

  1.交易所通过解析区块获得地址未花费交易(unspent)指定转入对象,调用命令如下：

```bash
	setuprawdeal [{"txid":"id","vout":n},...] {"address":amount,...} ( [data] "action" )
	--如果是链上发行的资产
	setuprawdeal [{"txid":"id","vout":n},...] {"address":{"资产名称":0.01}} ( [data] "action" )
```

注：如果是进行发行资产的转账，请包含一个含有原生资产（CXC）的UTXO作为手续费。

> 方法参数

| 参数      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| deals     | 交易集合                                                     |
| addresses | 地址：数量                                                   |
| data      | 附加数据                                                     |
| action    | 默认为“”,lock（锁定钱包中的给定输入），sign（使用钱包密钥签署交易），lock,sign，send（签署并发送交易 |

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
注意将手续费的vin拼入，

字节数的计算方式如下：

10+vin的个数 * 180+vout的个数 * 34

如果vin中包含发行资产

+10+（包含发行资产的vin的数量+包含发行资产的vout的数量） * 24

如果包含元数据（data）

+11+元数据16进制的字节数

手续费=字节数 * 0.0000001

2.通过addrawchange设置找零地址以及指定交易手续费

> 调用命令

```bash
	addrawchange "tx-hex" "address" ( fee )
```

> 方法参数

| 参数    | 描述     |
| ------- | -------- |
| tx-hex  | Hex      |
| address | 找零地址 |
| fee     | 手续费   |

> e.g.

```bash
	addrawchange "HEX""ADDR"
	addrawchange "HEX""ADDR" 0.01
```

> 返回值

```bash
	hex
```

建议交易所的找零地址单独设置，防止解析vout找零重复。

3.通过signrawdeal进行离线私钥签名交易

> 调用命令

```bash
	signrawdeal "tx-hex" ( [{"txid":"id","vout":n,"scriptPubKey":"hex","redeemScript":"hex"},...] ["privatekey1",...] sighashtype )
```

> 方法参数

| 参数        | 描述                                           |
| ----------- | ---------------------------------------------- |
| Tx-hex      | Hex                                            |
| prevtxs     | 交易数组，可设置为[]                           |
| privatekeys | 私钥数组，如果是在有私钥的节点签名，可设置为[] |
| sighashtype | 签署类型，默认为all                            |

> 返回值

```bash
	{
	  "hex": "value",                           The raw deal with signature(s) (hex-encoded string)
	  "complete": true|false                    If deal has a complete set of signature (0 if not)
	}
```

若complete为true，则签名成功，否则失败

4.通过sendrawdeal广播交易

> 调用命令

```bash
	sendrawdeal "tx-hex" ( allowhighfees )
```

> 方法参数

| 参数          | 描述                             |
| ------------- | -------------------------------- |
| Tx-hex        | 签名后要广播的hex                |
| allowhighfees | 默认为false,是否允许更高的手续费 |

> e.g.

```bash
	sendrawdeal "signedhex"
```

> 返回值

```bash
	txid
```

例：

```shell
  #向12c6DSiU4Rq3P4ZxziKxzrL5LmMBrzjrJX地址30个原生币(CXC)
	setuprawdeal '[{"txid":"526be568b3756124701e7d8c639dd3ffba1a40947cc2573fada996c1e08b4c89","vout":0},{"txid":"526be568b3756124701e7d8c639dd3ffba1a40947cc2573fada996c1e08b4c89","vout":1}]'  '{"12c6DSiU4Rq3P4ZxziKxzrL5LmMBrzjrJX":{"":30}}'
	#返回 dealhex
0100000002894c8be0c196a9ad3f57c27c94401abaffd39d638c7d1e70246175b368e56b520000000000ffffffff894c8be0c196a9ad3f57c27c94401abaffd39d638c7d1e70246175b368e56b520100000000ffffffff0180c3c901000000004f76a914119b098e2e980a229e139a9ed01a469e518e6f2688ac3473706b71ef28a6c20ae4fd378fbd6eed144bfcff80969800000000000e64ad8c6ebffc4d749dbd3e1f93090f002d3101000000007500000000
```

### 设置上一步Hex的找零地址12c6DSiU4Rq3P4ZxziKxzrL5LmMBrzjrJX 与 手续费 0.001

```shell
addrawchange 0100000002894c8be0c196a9ad3f57c27c94401abaffd39d638c7d1e70246175b368e56b520000000000ffffffff894c8be0c196a9ad3f57c27c94401abaffd39d638c7d1e70246175b368e56b520100000000ffffffff0180c3c901000000004f76a914119b098e2e980a229e139a9ed01a469e518e6f2688ac3473706b71ef28a6c20ae4fd378fbd6eed144bfcff80969800000000000e64ad8c6ebffc4d749dbd3e1f93090f002d3101000000007500000000 12c6DSiU4Rq3P4ZxziKxzrL5LmMBrzjrJX 0.001
#返回带找零地址的dealhex2
```

### 为交易进行签名并广播交易

```shell
#使用私钥privatekey1、privatekey2对hex进行签名
signrawdeal dealhex2 '[]' '["privatekey1","privatekey2"]'
#若在含有私钥的节点进行签名广播
sendrawdeal dealhex2 
返回签名后的hex与complete字段。当complete字段为true时 代表所有unspent均被签名可以进行广播
```

```json
{
    "hex" : "xxxxxxx",
    "complete" : true
}
```

## 相关命令

### 查询余额

  使用showallbals或者showaddrbals可查询用户余额,资产最小单位为0.000001。

### 查询交易信息

#### showdeal

> 方法说明

返回指定的钱包交易明细,此命令只可查询在本节点的地址的相关交易

> 调用命令

```bash
	showdeal txid
```

> e.g.

```bash
	showdeal  "txid"
```

> 返回值

```bash
{
    "amount" : "0.00",
    "fee" : "0.000069",
    "confirmations" : 10370,
    "blockhash" : "007191ede930cfxxxxxxxxxxxxxxxxxxxxxxxxxxxfd6a55d2d1802a78f4f51b2",
    "blockindex" : 4,
    "blocktime" : 1562300531,
    "txid" : "c71a6a6de7b4312ba9cd6e1e575861122491e6c17d9b74e6920989cab8c4404f",
    "walletconflicts" : [
    ],
    "valid" : true,
    "time" : 1562300524,
    "timereceived" : 1562300524,
    "details" : [
        {
            "account" : "",
            "address" : "13r6xxxxxxxxxxxxxxxxxxxxxxxxxx6p3snv2",
            "category" : "send",
            "amount" : "4.00",
            "vout" : 0,
            "fee" : "0.000069"
        },
        {
            "account" : "",
            "address" : "1JeZz3KS9xxxxxxxxxxxxxxxxxxxx68Syr3diH",
            "category" : "send",
            "amount" : "0.019771",
            "vout" : 1,
            "fee" : "0.000069"
        },
        {
            "account" : "",
            "address" : "13r6azRyVEXxxxxxxxxxxxxxxxxfM6p3snv2",
            "category" : "receive",
            "amount" : "4.00",
            "vout" : 0
        },
        {
            "account" : "",
            "address" : "1JeZz3KS9MAMYxxxxxxxxxxCEr68Syr3diH",
            "category" : "receive",
            "amount" : "0.019771",
            "vout" : 1
        }
    ],
    "hex" : "0100000004c154d448e92dc607xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx1f394bf385e213d3b1b56e07b73669336680454d88ac3b4d0000000000001976a914c1950fe62cc53640f6d449572a51ddc17b9e6ff988ac00000000"
}	
```

#### showrawdeal

> 方法说明

查询交易信息。

> 调用命令

```bash
	showrawdeal "txid" 
```

> e.g.

```bash
	showrawdeal "txid"
```

> 返回值

```bash
	hex 交易的完整hex信息
```

####  

### 设置节点手续费

  通过settxfee的方式可设置节点转账时的基础手续费，具体手续费还是根据字节收取手续费。

###  

### 设置节点归集

	为提高节点性能，当前节点会将属于同一地址的大量未使用输出（UTXO）自动组合成一个未使用的输出，默认数量是50。
	
	根据不同情况，可在节点运行时指定以下归集参数：

integminin    触发自动归集的最低未使用输出数量

integmaxin    一次归集交易的最大未使用输出数量

integinterval    两次自动归集之间的延迟时间，单位为秒


> e.g.

```bash
	cxcsz CXCChain -integminin=50 -integmaxin=100 -integinterval=100
```

### 节点加解密
	节点加解密详情参考https://github.com/cxcblock/doc/tree/master/developer

### 查看发行资产基本信息

通过以下命令可查看链上发行资产的基本信息

#### showassets

> 方法说明

返回链中已发行的资产的信息

> 调用命令

```bash
	showassets ( asset-id(s) detail count start )
```

> 方法参数

| 参数        | 描述                                                       |
| ----------- | ---------------------------------------------------------- |
| asset-id(s) | 资产名称或资产id或资产名称集合或资产id集合                 |
| detail      | true or false,默认为false,若为true，则显示资产更详细的信息 |
| count       | 显示数量,默认为INT_MAX                                     |
| start       | 默认为-count                                               |

> e.g.

```bash
	showassets
	showassets "AAA"
```

> 返回值

```bash
	一个资产集合
```

#### 

