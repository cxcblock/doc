# 1 CXCChain introduction

CXC public blockchain is the founding body. In its system, whether you are a merchant, an individual or a developer, you can rely on multi-layer information marketing intelligent
contract for fast fission as cells. DNA, that is CXC blockchain business
contract, can have continuous inheritance in the process of reproduction and evolve
through the contribution of collective intelligence. The whole system will
become more and more perfect, allowing participants to gradually achieve
financial freedom, social freedom, personality freedom, and experience the
shocking experience from single cell fission to human.

github:https://github.com/cxcblock

# 2.JSON-RPC API Instruction sets

## 2.1 JSON-RPC Overview of protocol

```
	JSON- RPC is a cross-language remote calling protocol based on json. Compared with  the text-based protocols such as xml-rpc, webservice and others, the data transmission grid is smaller; it is more easily to debug, implement and extend compared to the binary protocols such as hessian, java-rpc and others, and  is an excellent remote calling protocol. now,  the implementation framework of json-rpc is present for the mainstream language.
```

1.Transfer the following format to the blockchain node when invoking a remote call:

```json
json { "method": "showinfo", "configs": [], "id": 1}
```
> parameter description：

```
	 method:Name of method called
	 configs:imported parameter by Method  Array type.
	 id:Call identifier. It is used to mark a remote call procedure
```
2.Block chain node receives the call request, processes the method call, and gives the result effect of method utility to the caller; returns the data format.

```json
json {"result":	{"paytxfee":0,"relayfee":0.1,"rpcport":7318,"maxout":2000000},"error":null,"id":"1"}
```
> parameter description：

```
	result:method return value。
	error:Error in call, error-free return null。
	id:Call identifier，which is consistent with the identifier imported by the caller.
```
## 2.2 Command execution mode

RPC username and password are stored in the file of /.cxcs/CXCChain/cxcs.conf（linux/MAC）or %APPDATA%\CXcs\CXCChain\cxcs.conf（Windows）, which can be connected by using the cxcsi command tool or the CLI interface built into the CXCChain GUI tool (PC side wallet). These tools will automatically read the RPC username and password and connect to the running blockchain. Please do not open the rpc port to the external network.

> Run the command:

```bash
  [command] [configs]
```
## 2.3 Available API instruction

### 2.3.1query and control of blockchain

**The way to command  cxcsi CXCChain (command),For example cxcsi CXCChain help.

#### help
> description

Return to the list of available commands

> command

```bash
  help (command)
```
> parameter

| parameter | description                  |
| --------- | ---------------------------- |
| command   | Command name or non-paramter |

> e.g.

```bash
  help pause
```

> result

If there is no parameter to display the command list；if there is a parameter to display how to use this command

#### stop
> description

Stop blockchain operation

> command

```bash
  stop
```
> parameter

no

> result

CXcs server stopping

#### pause
> description

Pause blockchain

> command

```bash
  pause (task(s))
```
> parameter

| parameter | description                                                  |
| --------- | ------------------------------------------------------------ |
| task(s)   | The service which is to be stopped, the possible values are incoming,mining,outchain |

> e.g.

```bash
  pause incoming,mining
```

> result

Null

#### resume
> description

Restore the blockchain and use it with pause

> command

```bash
  resume (task(s))
```
> parameter

| parameter | description                                                  |
| --------- | ------------------------------------------------------------ |
| task(s)   | The service which is to restart, the possible values are incoming,mining,outchain |

> e.g.

```bash
  resume incoming,mining
```

> result

Null
#### showmem
> description

Return memory pool information 

> command

```bash
  showmem
```
> parameter

null

> result

```bash
  {
		"size" : 0,
		"bytes" : 0
	 }
```
#### addnode
> description

Manually add or remove point-to-point link

> command

```bash
  addnode "node" "add"|"remove"|"onetry"	
```
> parameter

| parameter | description |
| --------- | ----------- |
| node      | node        |
|command	|add add the node to a connection set; remove remove the node out of the set;onetry Try connecting node test|

>  e.g.

```bash
  addnode "ip:port" "add"
```
#### shownode
> description

Query connection node

> command

```bash
  shownode  ( "node" )
```
> parameter 

| parameter           | description                                                  |
| ------------------- | ------------------------------------------------------------ |
| (boolean)true/false | true= Return information about adding nodes using addnode，false= Return the list of nodes added using addnode |
|node	| the node that contains or omits ip(:port)	|

> 
> e.g.

```bash
  shownode true shownode true "ip"
```

> result

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
> description

Return the connected network IP and port information

> command

```bash
  shownet
```
> parameter

null


> result

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
> description

Return information about other connected nodes

> command

```bash
  showpeer
```
> parameter

null

> result

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
> description

Show chain information

> command

```bash
  showchain
```
> parameter

null

> result

```bash
  {
		"blocks" : 2016,
		"headers" : 2016,
		"bestblockhash" : "000061d0db856cf824e6e20e0eeb4005ac968a145b0aa142619a0127d073bdc2"
	}
```
#### showblock
> description

View the block information of the specified hash|height

> command

```bash
  showblock "hash"|height ( type )
```
> parameter

| parameter   | description                |
| ----------- | -------------------------- |
| hash/height | Block hash or block height |
|type	|0- encoded data, 1- json object, 2- with tx encoded data, 4- with tx json object	|

> e.g.

```bash
  showblock "105"
```

> result

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
> description

Return information about the specified block

> command

```bash
  showblocks block-set-identifier ( false|true )
```
> parameter

| parameter            | description                                                  |
| -------------------- | ------------------------------------------------------------ |
| block-set-identifier | Block height or block hash / Height set or time range {"starttime" : start-time Start time."endtime" : end-time End time.} |
|false/true	| the default is false. If true, the more detailed information will be shown.	|

e.g.

```bash
  showblocks "hash1,hash2"
	showblocks "hash"
```

> result

```bash
  Block information set
```
#### showblockhash
> description

Return the HASH value of the specified height block

> command

```bash
  showblockhash index
```
> parameter 

| parameter | description  |
| --------- | ------------ |
| index     | Block height |

e.g.

```bash
  showblockhash 20
```

> result

```bash
  Block hash value
```
#### signmessage
> description

Sign for messages based on private keys

> command

```bash
  signmessage "address|privkey" "message"
```
> parameter

| parameter       | description                                                  |
| --------------- | ------------------------------------------------------------ |
| address/privkey | Address or private key, if it is an address, ensure that the private key of the address is also on the node. |
|message	| Message to be signed	|

e.g.

```bash
  signmessage "addr" "my message"
```

> result

```bash
  the string after signing
```
#### checkmessage
> description

verification message

> command

```bash
  checkmessage "publickey" "signature" "message"
```
> parameter

| parameter | description |
| --------- | ----------- |
| publickey | Public key  |
|signature	| the string after signing	|
|message | Message before signing	|


> result

```bash
  true|false
```
### 4.3.2 Manage wallet address key
#### addnewaddr
> description

Create a new address

> command

```bash
  addnewaddr
```
> parameter

null

> result

```bash
  the created new address
```
#### addmultiaddr
> description

create the address for multiple signatures and add it to the node

> command

```bash
  addmultiaddr nrequired keys
```
> parameter

| parameter | description                                            |
| --------- | ------------------------------------------------------ |
| nrequired | the number of signature addresses required for signing |
|keys	| Public key set	|

> e.g.

```bash
  addmultiaddr 2 "[\"HEXPUBKEYS\",\"HEXPUBKEYS\"]"
```

> result

```bash
  Multi-sign address
```
#### setupmulti
> description

Create the multi-sign address but not add to the wallet, it is not recommended to use

> command

```bash
  setupmulti nrequired keys
```
> parameter

| parameter | description                                            |
| --------- | ------------------------------------------------------ |
| nrequired | The number of signature addresses required for signing |
|keys	|Public key set	|

> e.g.

```bash
  setupmulti 2 "[\"HEX-PUBKEYS\",\"HEX-PUBKEYS\"]"
```

> result

```bash
  {
	  "address":"multisigaddress",          The value of the new multisig address.
	  "redeemScript":"script"               The string value of the hex-encoded redemption script.
	}
```
#### setupkeypairs
> description

Generate one or more public/private key pairs，the default is 1

> command

```bash
  setupkeypairs ( count )
```
> parameter

| parameter | description                                                  |
| --------- | ------------------------------------------------------------ |
| count     | the number of public/private key pairs created，the default is 1 |


> e.g.

```bash
  setupkeypairs 10
```

> result

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
> description

Return the detailed information of address in this node wallet

> command

```bash
  showaddrs ( addr(es) count start )
```
> parameter 

| method   | parameter                                                    |
| -------- | ------------------------------------------------------------ |
| addr(es) | （Show all addresses）or addr(single address) Or address array，the default is |
| count    | Show the number of addresses, the default is INT_MAX         |
| start    | Start from which article , the default is -count             |


> e.g.

```bash
  showaddrs
	showaddrs "*"
	showaddrs "ADDR"
```

> result

```bash
  Array of address objects
```
#### dumpprivkey
> description

Export private key

> command

```bash
  dumpprivkey "address"
```
> parameter

| parameter | description                                   |
| --------- | --------------------------------------------- |
| address   | The address of the private key to be exported |

 


> result

```bash
  Private key
```
#### importprivkey
> description

Import private key

> command

```bash
  importprivkey privkey(s)
```
> parameter

| parameter  | description                                                  |
| ---------- | ------------------------------------------------------------ |
| privkey(s) | Required, array of single private key or private key，       |
| rescan     | Whether to rescan the transaction of the wallet, the default is true |

> e.g.

```bash
  importprivkey "PRIKEY"
```

> result

```bash
  No
```
#### importaddr
> description

Add address to the wallet (without private key), create one or more accounts for read-only

> command

```bash
  importaddr address(es)
```
> parameter

| parameter   | description                                                  |
| ----------- | ------------------------------------------------------------ |
| address(es) | Required, single address or address array                    |
| rescan      | Whether to rescan the transaction of the wallet, the default is true |


> e.g.

```bash
  importaddr "ADDR"
```

> result

```bash
  No
```
#### backupwallet
> description

Backup wallet.dat Wallet file

> command

```bash
  backupwallet "destination"
```
> parameter

| parameter   | description |
| ----------- | ----------- |
| destination | file path   |


> e.g.

```bash
  backupwallet "backup.dat"
```

> result

```bash
  No
```
#### dumpwallet
> description

Dump all private keys in the wallet to text file

> command

```bash
  dumpwallet "filename"
```
> parameter

| parameter | description            |
| --------- | ---------------------- |
| filename  | File path to export to |


> e.g.

```bash
  dumpwallet "test"
```

> result

```bash
  No
```
#### encryptwallet
> description

the wallet for the encrypted node for the first time, use password as the unlock password Please note that after encryption, you cannot restore to non-password state, unless you delete all files of the node and resynchronize the block！

> command

```bash
  encryptwallet "password"
```
> parameter

| parameter | description |
| --------- | ----------- |
| password  | password    |


> e.g.

```bash
  encryptwallet "password"
```

> result

```bash
  No
```
Note that once the node is encrypted, whether it is to restart the node or change the password, you need to call the resume incoming, mining command.
#### changepass
> description

Change wallet password

> command

```bash
  changepass password newpassword
```
> parameter

| parameter   | description  |
| ----------- | ------------ |
| password    | old password |
| newpassword | new password |


> result

```bash
  No
```
#### walletpass
> description

use password to unlock the node wallet, which is valid for the time.

> command

```bash
  walletpass password time
```
> parameter

| parameter | description                         |
| --------- | ----------------------------------- |
| password  | password                            |
| time      | Unlock time with the unit of second |


> e.g.

```bash
  walletpass password 60
```

> result

```bash
  true|false
```
#### showassets
> description

Return information about the assets that have been issued in the chain

> command

```bash
  showassets ( asset-id(s) detail count start )
```
> parameter

| parameter   | description                                                  |
| ----------- | ------------------------------------------------------------ |
| asset-id(s) | Asset name or asset id or set of asset name or set of asset id |
| detail      | true or false, the fault is false, if true，show more detailed information about the asset |
| count       | Display the quantity, the default is INT_MAX                 |
| start       | the default is -count                                        |


> e.g.

```bash
  showassets
	showassets "AAA"
```

> result

```bash
  an assets set
```
#### showaddrbals
> description

return to the list of all asset balances in the wallet of this account node

> command

```bash
  showaddrbals "address" ( minconf false|true )
```
> parameter

| method     | description                                                  |
| ---------- | ------------------------------------------------------------ |
| address    | address query                                                |
| minconf    | minimum block confirmation value                             |
| false/true | false by default. If it is true, read only accounts are contained |


> e.g.

```bash
  showaddrbals "ADDR"
	showaddrbals "ADDR" 0 true
```

> result

```bash
  balance of each address
```
#### showallbals
> description

return to the list of all asset balances in the wallet of this account node designated by the address(including the underlying currency)

> command

```bash
  showallbals ( address(es) assets minconf false|true false|true )
```
> parameter

| parameter   | description                                                  |
| ----------- | ------------------------------------------------------------ |
| address(es) | query the address or address set of the balance              |
| assets      | queried assets or assets set                                 |
| minconf     | minimum block confirmation value                             |
| false/true  | false by default. If it is true, read only accounts are contained |


> e.g.

```bash
  showallbals
	showallbals "address"
	showallbals "address" "AAA"
```

> result

```bash
  return to balance
	
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
> description

return to the transaction records of this account address

> command

```bash
  showaddrdeal "address" "txid"
```
> parameter

| parameter | description |
| --------- | ----------- |
| address   | address     |
| txid      | The TXID    |


> e.g.

```bash
  showaddrdeal "address" "txid"
```

> result

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
> description

return to the transaction information of this account address

> command

```bash
  showaddrdeals "address" ( count skip false|true )
```
> parameter

| parameter  | description                                              |
| ---------- | -------------------------------------------------------- |
| address    | address                                                  |
| count      | 10 by default. Show the number of records                |
| skip       | 0 by default. Record the subscript of the start of query |
| false/true | false by default. If it is true, show more detail item   |


> e.g.

```bash
  showaddrdeals "ADDR"
	showaddrdeals "ADDR" 20 100
```

> result

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
> description

rovides information about transactions of the wallet of the node txid

> command

```bash
  showwalletdeal "txid" ( false|true false|true )
```
> parameter

| parameter  | description                                                  |
| ---------- | ------------------------------------------------------------ |
| txid       | txid                                                         |
| false/true | false by default. If it is true, only read accounts are contained |


> e.g.

```bash
  showwalletdeal "TXID"
	showwalletdeal "TXID" true
```

> result

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
#### showwalletdeals
> description

query about transactions of related wallet

> command

```bash
  showwalletdeals ( count skip false|true)
```
> parameter

| parameter  | description                                                  |
| ---------- | ------------------------------------------------------------ |
| count      | number of transaction records                                |
| skip       | subscript of start, 0 by default                             |
| false/true | false by default. If it is true, read only accounts are contained |


> e.g.

```bash
  showwalletdeals
	showwalletdeals 20 100 true
```

> result

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
> description

query transaction details based on asset id and txid

> command

```bash
  showassetdeal "asset-id" "txid" ( false|true )
```
> parameter

| parameter  | description                                           |
| ---------- | ----------------------------------------------------- |
| asset-id   | asset id or asset name                                |
| txid       | txid                                                  |
| false/true | false by default. If it is true,show more detail item |


> e.g.

```bash
  showassetdeal "myasset" "mytxid"
	showassetdeal "myasset" "mytxid"  true
```

> result

```bash
  transaction details
	
	"deal"                              Info about an individual deal from the perspective of a particular asset.
```
#### showassetdeals
> description

query the transaction information of a specific asset

> command

```bash
  showassetdeals "asset-id" ( count start local-ordering )
```
> parameter

| parameter      | description                                                  |
| -------------- | ------------------------------------------------------------ |
| asset-id       | asset id or asset name                                       |
| count          | current show	,10 by default                               |
| start          | starting count,-count by default                             |
| local-ordering | false by default.false indicates query from wallet and true indicates query from chain |


> e.g.

```bash
  showassetdeals "test-asset"
	showassetdeals "test-asset"  10 100 true
```

> result

```bash
  List of deals
```
#### send
> description

send one or more assets

> command

```bash
  send "address" object ( "comment" "comment-to" )
```
> parameter

| parameter  | description                                                  |
| ---------- | ------------------------------------------------------------ |
| address    | transfer address                                             |
| object     | if the quantity is directly filled in, the underlying asset will be sent. Or fill in {"asset-name" : asset-count,"": nativecurrency-count Native currency use "",...} |
| comment    | additional information, not on block chain                   |
| comment-to | attribution of additional information, not on the block chain |


> e.g.

```bash
  send "ADDRTO" "{\"assetname\":1000,\"\":2000}"
```

> result

```bash
  txid
```
#### sendfrom
> description

transfer to designated address

> command

```bash
  sendfrom "from-address" "to-address" quantities-object ( "comment" "comment-to" )
```
> parameter

| parameter         | description                                                  |
| ----------------- | ------------------------------------------------------------ |
| from-address      | originating address                                          |
| to-address        | target address                                               |
| quantities-object | if the quantity is directly filled in, the underlying asset will be sent. Or fill in {"asset-name" : asset-count,"": nativecurrency-count Native currency use "",...} |
| comment           | additional information, not on block chain                   |
| comment-to        | attribution of additional information, not on the block chain |


> e.g.

```bash
  sendfrom "ADDRFROM" "ADDRTO" "{\"assetname\":1000,\"\":2000}"
	sendfrom "ADDRFROM" "ADDRTO" 0.1 "desc" "desc out"
```

> result

```bash
  txid
```
#### sendasset
> description

send a certain amount of assets to a specified address

> command

```bash
  sendasset "address" "asset-id" count ( native-amount "comment" "comment-to" )
```
> parameter

| parameter     | description                                                  |
| ------------- | ------------------------------------------------------------ |
| address       | address                                                      |
| asset-id      | asset id or asset name                                       |
| count         | quantity of assets                                           |
| native-amount | quantity of underlying assets                                |
| comment       | additional information, not on block chain                   |
| comment-to    | attribution of additional information, not on the block chain |


> e.g.

```bash
  sendasset "ADDRTO" assetname 1000
	sendasset "ADDRTO" assetname 1000 0.1 "desc" "desc out"
```

> result

```bash
  txid
```
#### sendassetfrom
> description

transfer to a desiganted account

> command

```bash
  sendassetfrom "from-address" "to-address" "asset-id" asset-count ( native-amount "comment" "comment-to" )
```
> parameter

| parameter     | description                                                  |
| ------------- | ------------------------------------------------------------ |
| from-address  | transfer address                                             |
| to-address    | target address                                               |
| asset-id      | asset id or name                                             |
| asset-count   | quantity of assets                                           |
| native-amount | quantity of underlying assets                                |
| comment       | additional information, not on block chain                   |
| comment-to    | attribution of additional information, not on the block chain |


> e.g.

```bash
  sendassetfrom "ADDRFROM" "ADDRTO" 0.1 assetname 1000
	sendassetfrom "ADDRFROM" "ADDRTO" 0.1 assetname 1000 "desc" "desc out"
```

> result

```bash
  txid
```
#### senddata
> description

similar send, additional information

> command

```bash
  senddata "address" amount|asset-quantities data|send-new-item
```
> parameter

| parameter        | description                                                  |
| ---------------- | ------------------------------------------------------------ |
| address          | target address                                               |
| asset-quantities | quantity of underlying assets or asset obj                   |
| data             | data, including hex,text,json. View the data that can be transmitted through help with-data |


> e.g.

```bash
  senddata "ADDR" "{\"11111-2222-5678\":1000,\"22222-3333-1234\":2000}" data-hex
	senddata "ADDR" 0.1 data-hex
```

> result

```bash
  txid
```
#### senddatafrom
> description

transfer to a desiganted account with data。

> command

```bash
  senddatafrom "from-address" "to-address" amount|asset-qty data|send-new-item
```
> parameter

| parameter        | description                                                  |
| ---------------- | ------------------------------------------------------------ |
| from-address     | originating address                                          |
| to-address       | target address                                               |
| asset-quantities | quantity of underlying assets or asset obj                   |
| data             | data, including hex,text,json. View the data that can be transmitted through help with-data |

> e.g.

```bash
  senddatafrom "ADDR" "ADDR" "{\"key\":100,\"key2\":200}" data-hex
	senddatafrom "ADDR" "ADDR" 0.1 data-hex
```

> result

```bash
  txid
```
#### validaddr
> description

verify whether the address is correct

> command

```bash
  validaddr "address"|"pubkey"|"privkey"
```
> parameter

| parameter              | description                          |
| ---------------------- | ------------------------------------ |
| address/pubkey/privkey | address or public key or private key |


> e.g.

```bash
  validaddr "ADDR"
```

> result

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
### 4.3.3 create assets for issuance /ChainDB

#### sell
> description

issue a certain amount of assets to the address

> command

```bash
  sell "address" "asset-name"|asset-configs count ( unit native-amount extend-data )
```
> parameter

| parameter     | description                                                  |
| ------------- | ------------------------------------------------------------ |
| address       | address                                                      |
| asset-configs | asset name or asset parameter {"name":"asset1","open":true},if open is true,additional issue can be made |
| count         | issuance number                                              |
| unit          | minimum unit, 1 by default                                   |
| native-amount | quantity of underlying assets                                |
| extend-data   | additional information                                       |


> e.g.

```bash
  sell "ADDR" "Asset1" 500
	sell "ADDR" "Asset1" 500 0.01
	sell "ADDR" '{"name":"asset1","open":true}' 500
```

> result

```bash
  txid
```
#### sellfrom
> description

issue from specific address

> command

```bash
  sellfrom "from-address" "to-address" "asset-name"|asset-configs qty ( unit native-amount custom-fields )
```
> parameter

| parameter     | description                                                  |
| ------------- | ------------------------------------------------------------ |
| from-address  | originating address                                          |
| to-address    | target address                                               |
| asset-configs | asset name or asset parameter{"name":"asset1","open":true},if open is true, additional issue can be made |
| count         | issuance number                                              |
| unit          | minimum unit, 1 by default                                   |
| native-amount | quantity of underlying assets                                |
| extend-data   | additional information                                       |

> e.g.

```bash
  sellfrom "ADDRFROM" "ADDRTO" "Asset1" true 1000
	sellfrom "ADDRFROM" "ADDRTO" Asset1  1000 0.01 0.01
```

> result

```bash
  txid
```
#### sellasset
> description

issue a certain amount of additional assets to the designated address.

> command

```bash
  sellasset "address" "asset-id" qty ( native-amount custom-fields )
```
> parameter

| parameter     | description                            |
| ------------- | -------------------------------------- |
| address       | target address                         |
| asset-id      | asset id or asset name                 |
| qty           | quantity of additional issuance        |
| native-amount | quantity of underlying assets          |
| custom-fields | self-defined parameter,additional data |

> e.g.

```bash
  sellasset "ADDR" "Asset1" 1000
	sellasset "ADDR" Asset1 1000 0.01
```

> result

```bash
  txid
```
#### sellassetfrom
> description

issue a certain amount of additional assets to the designated address from a specific address

> command

```bash
  sellassetfrom "from-address" "to-address" "asset-id" qty ( native-amount custom-fields )
```
> parameter

| parameter     | description                            |
| ------------- | -------------------------------------- |
| from-address  | originating address                    |
| to-address    | target address                         |
| asset-id      | asset id or asset name                 |
| qty           | quantity of additional issuance        |
| native-amount | quantity of underlying assets          |
| custom-fields | self-defined parameter,additional data |

> e.g.

```bash
  sellassetfrom "Manage ADDR" "ADDRTO" "Asset1" 1000
	sellassetfrom "manage address" "ADDRTO" Asset1 1000 0.01
```

> result

```bash
  txid
```
#### setupdatamod
> description

create ChainDB. ChainDB is stored in the block chain in the form of key-value

> command

```bash
  setupdatamod   "name" access  (extend-data )
```
> parameter

| parameter   | description                                     |
| ----------- | ----------------------------------------------- |
| name        | name                                            |
| access      | true/false whether anyone is permitted to write |
| extend-data | additional information                          |


> e.g.

```bash
  setupdatamod datatest false
	setupdatamod datatest false '{"desc":"Test Data"}'
```

> result

```bash
  txid
```
#### setupdatamodfrom
> description

create with specific address ChainDB

> command

```bash
  setupdatamodfrom "from-address"   "name" access  (extend-data )
```
> parameter

| parameter    | description                                     |
| ------------ | ----------------------------------------------- |
| from-address | originating address                             |
| name         | name                                            |
| access       | true/false whether anyone is permitted to write |
| extend-data  | additional information                          |

> e.g.

```bash
  setupdatamodfrom "ADDRFROM" dm1  false
	setupdatamodfrom "ADDRFROM" dm2  false '{"desc":"info"}'
```

> result

```bash
  txid
```
#### senditem
> description

write data in ChainDB

> command

```bash
  senditem "datamodId" "key"|keys "data-hex"|data-obj "options"
```
> parameter

| parameter | description                                                  |
| --------- | ------------------------------------------------------------ |
| datamodId | DBname or id                                                 |
| key/keys  | key value or key value array                                 |
| data      | data, including hex,text,json. View the data form that can be supported through help with-data |
| options   | not fill or outchain                                         |

> e.g.

```bash
  senditem test "text" data
```

> result

```bash
  txid
```
#### senditemfrom
> description

write in ChainDB according to specific address

> command

```bash
  senditemfrom "from-address" "datamodid" "key"|keys "data-hex"|data-obj "options"
```
> parameter

| parameter    | description                                                  |
| ------------ | ------------------------------------------------------------ |
| from-address | originating address                                          |
| datamodId    | DBname or id                                                 |
| key/keys     | key value or key value array                                 |
| data         | data, including hex,text,json. View the data form that can be supported through help with-data |
| options      | not fill or outchain                                         |

> e.g.

```bash
  senditemfrom "fromaddr" datamodname "key" hex
```

> result

```bash
  txid
```
#### showdatas
> description

return to information about ChainDB created on the block chain

> command

```bash
  showdatas ( DatamodId(s) false|true count start )
```
> parameter

| parameter    | description                                            |
| ------------ | ------------------------------------------------------ |
| DatamodId(s) | DBname or id or set                                    |
| false/true   | false by default. If it is true, show more detail item |
| count        | INT_MAX by default,current show                        |
| start        | start,-count	by default                             |


> e.g.

```bash
  showdatas
```

> result

```bash
  ChainDB set
```
#### order
> description

subscribe DB（content that can only be DB queried if subscribed）

> command

```bash
  order entity-id(s) ( rescan )
```
> parameter

| parameter    | description                                                  |
| ------------ | ------------------------------------------------------------ |
| entity-id(s) | DB id or DB name                                             |
| rescan       | whether to re-scan the transactions in wallet. True by default |


> e.g.

```bash
  order "test-datamod" false
```

> result

```bash
  No
```
#### noorder
> description

unsubscribe DB

> command

```bash
  noorder entity-id(s) ( purge )
```
> parameter

| parameter    | description                                                  |
| ------------ | ------------------------------------------------------------ |
| entity-id(s) | DB name or set                                               |
| purge        | true or false false by default,clear all external link data stored in the data store |


> e.g.

```bash
  noorder "datamod1"
```

> result

```bash
  No
```
#### showdataitem
> description

query Chain DB information

> command

```bash
  showdataitem "datamod-id" "txid" ( detail )
```
> parameter

| parameter  | description                                      |
| ---------- | ------------------------------------------------ |
| datamod-id | Chain DBname orChain DB                          |
| TXID       | TXID                                             |
| detail     | false by default. If it is true,show detail item |


> e.g.

```bash
  showdataitem "DATAMODID" "TXID"
	showdataitem "DATAMODID" "TXID"  true
```

> result

```bash
  Chain DB data information
```
#### showdataitems
> description

query Chain DB set

> command

```bash
  showdataitems "datamodId" ( count start false|true )
```
> parameter

| parameter  | description                                  |
| ---------- | -------------------------------------------- |
| datamodId  | ChainDB name                                 |
| count      | current show,10 by default                   |
| start      | number from back to front, -count by default |
| false/true | false by default. If it is true              |

> e.g.

```bash
  showdataitems "datamod1" 10 100
```

> result

```basChainDB集合

```
#### showdatakeys
> description Keyword information of ChainDB

> command

```bash
  showdatakeys "DatamodId" ( key(s) count start false|true )
```
> parameter

| parameter  | description                                                  |
| ---------- | ------------------------------------------------------------ |
| DatamodId  | ChainDB name                                                 |
| key(s)     | by default,key value or key value set                        |
| count      | INT_MAX by default,current show                              |
| start      | -count by default                                            |
| false/true | false by default, true,data in the wallet,false,data in the block chain |


> e.g.

```bash
  showdatakeys "datamod1"
	showdatakeys "datamod1" "key01"
	showdatakeys "datamod1" "*" 10 100
```

> result

```bash
  key value array
	
	[
		{
			"key":"key01",
			"items":1,
			"confirmed":1
		}
	]
```
#### showdatakeyitems
> description

according to key ChainDB

> command

```bash
  showdatakeyitems "datamodid" "key" (count start false|true )
```
> parameter

| parameter  | description                                                  |
| ---------- | ------------------------------------------------------------ |
| datamodid  | ChainDBname or id                                            |
| key        | ChainDBkey value                                             |
| count      | 10 by default,current show                                   |
| start      | number from back to front, -count by default                 |
| false/true | if it is false,it is in the wallet; if it is true, it is in the block chain |


> e.g.

```bash
  showdatakeyitems "datamod1" "key01"
	showdatakeyitems "datamod1" "key01" 10 100
```

> result

```basChainDB数据集合
  Datamod item list
```
#### showdatasenderitems
> description

query according to the address of ChainDB

> command

```bash
  showdatasenderitems "datamodID" "address" ( false|true count start false|true )
```
> parameter

| parameter  | description                                                  |
| ---------- | ------------------------------------------------------------ |
| datamodId  | ChainDBname or id                                            |
| address    | address                                                      |
| false/true | false by default. If it is true,show detailed items,it is in the block chain |
| count      | 10 by default,current show                                   |
| start      | -count by default; count from back to front it is negative   |

> e.g.

```bash
  	showdatasenderitems "datamod" "address"
	showdatasenderitems "datamod" "address" true 10 100
```

> result

```basChainDB数据集合

```
#### showdatasenders
> description write the information of the address in ChainDB

> command

```bash
  showdatasenders "datamodid" ( address(es) false|true count start false|true )
```
> parameter

| parameter   | description                                                  |
| ----------- | ------------------------------------------------------------ |
| datamodid   | ChainDBname or id                                            |
| address(es) | by default, address or address set                           |
| false/true  | false by default. If it is true, show more detailed information of the sender |
| count       | INT_MAX by default,current show                              |
| start       | -count by default; number of count when starting query. If it is negative, query from back to front |
| false/true  | false by default; false, query the wallet; true, query from the block chain |


> e.g.

```bash
  showdatasenders "datamod1"
	showdatasenders "datamod1" address
	showdatasenders "datamod1" "*" true 10 100
```

> result

```bash
  data list of the sender
```
#### showtxoutdata
> description

query the additional information of specific transaction

> command

```bash
  showtxoutdata "txid" vout ( count-bytes start-byte )
```
> parameter

| parameter   | description                                 |
| ----------- | ------------------------------------------- |
| txid        | txid                                        |
| vout        | vout                                        |
| count-bytes | INT_MAX by default, query number of bytes   |
| start-byte  | 0 by default, number of bytes when starting |


> e.g.

```bash
  showtxoutdata "TXID" 1
```

> result

```bash
  additional information,including data of structures like hex,text,json
```
### atomic transaction

realize commands involved in the atomic transactions on the block chain.

#### prelock
> description

Lock the unspent output for atomic transactions.

> command

```bash
  prelock asset-qty ( true|false )
```
> parameter

| parameter  | description                                                  |
| ---------- | ------------------------------------------------------------ |
| asset-qty  | {"asset-id":qty}asset name: quantity                         |
| true/false | The default is true. If it is true, the assets are locked. If it is false, the assets are not locked. |


> e.g.

```bash
  prelock "{\"11111-2222-5678\":1000}"
	prelock "{\"11111-2222-5678\":1000,\"22222-3333-1234\":2000}"
```

> result

```bash
  {
	  "txid": "TXID",                       TXID of the output which can be spent in setuprawex or setuprawex
	  "vout": n                             VOUT index
	}
```
#### prelockfrom
> description

Specify the source address for asset locking.

> command

```bash
  	prelockfrom "from-address" asset-quantities ( lock )
```
> parameter

| parameter        | description                                                  |
| ---------------- | ------------------------------------------------------------ |
| from-address     | Lock the address                                             |
| asset-quantities | {"asset-id":qty}asset name: quantity                         |
| true/false       | The default is true. If it is true, the assets are locked. If it is false, the assets are not locked. |

> e.g.

```bash
  prelockfrom "ADDR" "{\"11111-2222-5678\":1000}"
	prelockfrom "ADDR" "{\"11111-2222-5678\":1000,\"22222-3333-1234\":2000}"
```

> result

```bash
  {
	  "txid": "TXID",                            TXID of the output which can be spent in setuprawex or setuprawex
	  "vout": n                                  VOUT index
	}
```
#### setuprawex
> description

Create a new atomic exchange trade for a given number of assets based on TXID and VOUT generated by the prelock operation, and return to a HEX text block TX-HEX for the other party to trade.

> command

```bash
  setuprawex "txid" vout ask-assets
```
> parameter

| parameter  | description                                    |
| ---------- | ---------------------------------------------- |
| txid       | txid generated by prelock                      |
| vout       | vout generated by prelock                      |
| ask-assets | Ask for the quantity of assets to be exchanged |


> e.g.

```bash
  setuprawex TXID 1 "{\"ASSETID2\":100}"
```

> result

```bash
  txid
```
#### decoderawex
> description

Decode tx-hex created by setuprawex.

> command

```bash
  decoderawex "tx-hex" ( false|true )
```
> parameter

| parameter  | description                                                  |
| ---------- | ------------------------------------------------------------ |
| tx-hex     | hex to be parsed                                             |
| false/true | The default is false. If it is true, more detailed information is shown |


> e.g.

```bash
  decoderawex "HEX-STR"
```

> result

```bash
	#Exchange list data
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
> description

Accept full offer/part of offer for atomic transaction

> command

```bash
  addrawex "hex" "txid" vout ask-assets
```
> parameter

| parameter  | description                                                |
| ---------- | ---------------------------------------------------------- |
| hex        | returned value of setuprawex or returned value of addrawex |
| txid       | returned value of prelock                                  |
| vout       | returned value of prelock                                  |
| ask-assets | assets accepted and quantity                               |


> e.g.

```bash
  addrawex "HEX-STR" TXID 1 "{\"1234-5678-9090\":200}"
```

> result

```bash
  {
	  "hex": "value",                    The raw deal with signature(s) (hex-encoded string)
	  "complete": true|false             If exchange is completed and can be sent
	}
```
#### completerawex
> description

Complete a transaction for exchange. Additional service charges can be charged if necessary

> command

```bash
  completerawex hex txid vout ask-assets ( data|send-new-item )
```
> parameter

| parameter          | description                  |
| ------------------ | ---------------------------- |
| hex                | hex                          |
| txid               | txid                         |
| vout               | vout                         |
| ask-assets         | assets received and quantity |
| data/send-new-item | data carried                 |


> e.g.

```bash
  completerawex "hexstring" txid 1 "{\"1234-5678-1234\":200}"
```

> result

```bash
  hex
```
#### sendrawdeal
> description

Verify atomic transaction and make broadcast transaction.

> command

```bash
  sendrawdeal "tx-hex" ( allowhighfees )
```
> parameter

| parameter     | description                                              |
| ------------- | -------------------------------------------------------- |
| tx-hex        | tx-hex                                                   |
| allowhighfees | The default is false. Is higher handling charge allowed? |


> e.g.

```bash
  sendrawdeal "signedhex"
```

> result

```bash
  txid
```
#### disrawdeal
> description

Disable quotation TX-HEX for exchange

> command

```bash
  disrawdeal "tx-hex"
```
> parameter

| parameter | description |
| --------- | ----------- |
| tx-hex    | hex         |


> result

```bash
  txid
```
#### gatherunspent
> description

Collect unused output (UTXO) and combine it into an unused output. Return a list of TXID, which improve wallet performance

> command

```bash
  gatherunspent ( "address(es)" minconf maxcombines mininputs maxinputs maxtime )
```
> parameter

| parameter   | description                                                  |
| ----------- | ------------------------------------------------------------ |
| address(es) | The default is *. The address or address set to be collected |
| minconf     | The default is 1. Minimum confirmed quantity                 |
| maxcombines | The default is 100. Maximum quantity of transactions collected |
| mininputs   | The default is 2. Minimum input quantity                     |
| maxinputs   | The default is 100. Maximum input quantity                   |
| maxtime     | The default is 15s. Maximum broadcast time                   |


> e.g.

```bash
  gatherunspent "*" 1 15 2 10 100
	gatherunspent "addr"
```

> result

```bash
  	txids
```
#### showlock
> description

Return to the list of unused transaction output lock in the wallet

> command

```bash
  showlock
```
> parameter

Null


> result

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
> description

Lock or unlock output not spent

> command

```bash
  lock unlock [{"txid":"txid","vout":n},...]
```
> parameter

| parameter | description                          |
| --------- | ------------------------------------ |
| unlock    | Lock false and unlock true           |
| deals     | transaction to be locked or unlocked |


> e.g.

```bash
  lock false "[{\"txid\":\"txid\",\"vout\":1}]"
	lock true "[{\"txid\":\"txid\",\"vout\":1}]"
```

> result

```bash
  true or false,Do you lock or unlock successfully
```
#### setuprawdeal

> description

Create a transaction that spends specified input and send it to the given address

> command

```bash
	setuprawdeal [{"txid":"id","vout":n},...] {"address":amount,...} ( [data] "action" )
```

> parameter

| parameter | description                                                  |
| --------- | ------------------------------------------------------------ |
| deals     | transaction set                                              |
| addresses | address: quantity                                            |
| data      | additional data                                              |
| action    | The default is “”,lock (lock the given input in the wallet), sign (sign the transaction by using the wallet key)，lock,sign，send (sign and send the transaction |





> e.g.

```bash
	setuprawdeal "[{\"txid\":\"myid\",\"vout\":0}]" "{\"address\":0.01}"
```

> returned value

```bash
	#If action is ""or lock or send
	hex
	#If action is of several other kinds
	{                                                        
	  "hex": "value",                                        The raw deal with signature(s) (hex-encoded string)
	  "complete": true|false                                 If deal has a complete set of signature (0 if not)
	}
```

#### setuprawsendfrom

> description

Creates a transaction by using the specified address

> command

```bash
	setuprawsendfrom "from-address" {"address":amount,...} ( [data] "action" )
```

> parameter

| parameter | description                                                  |
| --------- | ------------------------------------------------------------ |
| from-addr | specified address                                            |
| addresses | address: quantity                                            |
| data      | additional data                                              |
| action    | The default is“”,lock (lock the given input in the wallet), sign (sign the transaction by using the wallet key)，lock,sign，send (sign and send the transaction) |



> e.g.

```bash
	setuprawsendfrom "FROMADDR" "{\"ADDR\":0.01}"
```

> returned value

```bash
	Same to setuprawdeal
```

#### decoderawdeal

> description

Decode TX-HEX of transaction

> command

```bash
	decoderawdeal "tx-hex"
```

> parameter

| parameter | description      |
| --------- | ---------------- |
| tx-hex    | hex to be parsed |

 

> returned value

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

> description

Similar to setuprawdeal. Instead of creating a new transaction, add the given input and (regular or metadata) output to TX-HEX in the specified original transaction

> command

```bash
	addrawdeal "tx-hex" [{"txid":"id","vout":n},...] ( {"address":amount,...} [data] "action" )
```

> parameter

| parameter    | description                                                  |
| ------------ | ------------------------------------------------------------ |
| tx-hex       | Create hex after transaction                                 |
| transactions | transaction set                                              |
| addresses    | assets: quantity Can inquire through help addr-all           |
| data         | additional data                                              |
| action       | The default is“”,lock (lock the given input in the wallet), sign (sign the transaction by using the wallet key)，lock,sign，send (sign and send the transaction) |



> e.g.

```bash
	addrawdeal "hexstring" "[{\"txid\":\"myid\",\"vout\":0}]" "{\"address\":0.01}"
```

> returned value

```bash
	hex
```

#### addrawchange

> description

Accept the output of setuprawdeal. Specify the change address and service charge

> command

```bash
	addrawchange "tx-hex" "address" ( fee )
```

> parameter

| parameter | description    |
| --------- | -------------- |
| tx-hex    | hex            |
| address   | change address |
| fee       | service charge |



> e.g.

```bash
	addrawchange "HEX""ADDR"
	addrawchange "HEX""ADDR" 0.01
```

> returned value

```bash
	hex
```

#### addrawdata

> description

Add a data for the original transaction output TX-HEX for setuprawdeal.

> command

```bash
	addrawdata tx-hex data
```

> parameter

| parameter | description |
| --------- | ----------- |
| tx-hex    | hex         |
| data      | data        |



> e.g.

```bash
	addrawdata "TX-HEX" data-hex
```

> returned value

```bash
	hex
```

#### signrawdeal

> description

Sign the original transaction

> command

```bash
	signrawdeal "tx-hex" ( [{"txid":"id","vout":n,"scriptPubKey":"hex","redeemScript":"hex"},...] ["privatekey1",...] sighashtype )
```

> parameter

| parameter   | description                      |
| ----------- | -------------------------------- |
| tx-hex      | hex                              |
| prevtxs     | trading array                    |
| privatekeys | private key array                |
| sighashtype | signing type. The default is ALL |



> e.g.

```bash
	signrawdeal "HEX-STR"
```

> returned value

```bash
	{
	  "hex": "value",                           The raw deal with signature(s) (hex-encoded string)
	  "complete": true|false                    If deal has a complete set of signature (0 if not)
	}
```

# 5.Course sample

## 5.1 Regular transaction sample

CXCChain uses UTXO model as the underlying stored data structure. Its full name is called Unspent Transaction output, which is the unused transaction output. The balance in the account is not represented by a number, but by all the UTXO associated with the current account in the current block chain network. CXCChain network has two types of assets. One is the local currency, which runs at the bottom of the block chain network and is generated by consensus algorithm. There is no symbol. It is marked as "" at the bottom. All asset transfer transactions need to consume native currency. It is named CXC for identification. The other kind is the issued assets, which are the digital assets issued by the project party based on CXCChain usually with relevant symbols. All accounts must guarantee a certain amount of CXC in the account during transfer transaction or transaction processing. Otherwise the transfer cannot be successful.

### 5.1.1 Create multiple new addresses

```bash
	addnewaddr
	addr1
	addnewaddr
	addr2
	addnewaddr
	addr3
```

### 5.1.2 Send native assets

```bash
	send addr1 10
	txid1
	sendfrom addr1 addr2
	txid2
```

### 5.1.3 Send non-native assets

```bash
	send addr1 '{"BBB":10}'
	txid1
	sendfrom addr1 addr2 '{"BBB":10}'
	txid2
```

### 5.1.4 Send many kinds of assets

```bash
	send addr1 '{"BBB":10,"AAA":20,"":10}'
	txid1
	sendfrom addr1 addr2 '{"BBB":10,"AAA":20,"":10}'
	txid2
```

## 5.2 Asset design and creation of asset samples

### 5.2.1 Issuance of assets

- Find an address that holds the rights to issue digital assets (addr1)
- Issue a digital asset called AAA, create 10,000 asset units, set the minimum unit as 0.01, issue to addr2, and issue additional assets

```bash
	sellfrom addr1 addr2 '{"name":"AAA","open":true}' 10000 0.01 
```

- Check situation of assets

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

### 5.2.2 Issue additional assets

Issue additional 12,000 AAA to addr2

```bash
	sellassetfrom addr1 addr2 AAA 12000 0.01
```

## 5.3 Atomic exchange design and creation of asset samples

### 5.3.1 Principle of atomic transaction

Any CXC transaction can have multiple inputs and outputs, and each transaction can involve a different address on the block chain. CXC provides a number of APIs and can easily make atomic transactions. One party first uses prelock (from) and setuprawex to create an exchange offer in which they specify the assets they provide and quantity, as well as the assets and quantity they require in return. In this offer the assets and quantities they provide and the assets and quantities they require in return are specified. This offer is represented as a partial transaction, with an input and an output signed by the issuer. And then partial transaction representing the offer can be sent to a particular party or distributed to anyone for review and acceptance. This communication can be made on the block chain, such as by using the function of posting on the block chain browser or by using any method (even email) required for de-link. Another participant who receives the invitation can use it to review decoderawex and decide whether to accept it or not. If so, they prelock (from) and completerawex their inputs and outputs, increase the transactions, provide the required assets and quantity, and require to provide assets and quantity. Then the final transaction will be balanced and can be broadcast to the network sendrawdeal. CXC also provides disrawdeal API to disable the offer after posting.

### 5.3.2 Atomic transaction process

1.Issue assets AAA and BBB. 

- Execute Node 1, find an address where assets can be created (addr1), and create an asset to issue to addr1

```bash
	sellfrom addr1 addr1 '{"name":"AAA","open":true}' 22000 0.01
```

- Execute Node 2, find an address where assets can be created (addr2), and create an asset to issue to addr2

```bash
	sellfrom addr2 addr2 BBB 5000 0.01
```

2..Make point-to-point atomic switching, execute a simple atomic switching, and switch Node 1 1000 AAA with Node 2 500 BBB.

- Node 2: Create a locked transaction output containing 500 BBB:

```bash
	prelockfrom addr2 '{"BBB":500, "":0.01}'
	
	{
      "txid" : "XXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
      "vout" : 0
	}
```

Get txid and vout

- Node 2: Create the transaction and specify the exchange 1000 AAA

```bash
	setuprawex b6bb4dbdf088a0d1cdcb6db451c1bd9de80b59e449488301627f5c706ce3bf11 0 '{"AAA":1000}'
	
	HEX1
```

Output a hexadecimal block of text containing the original transaction data representing the exchange offer. Text block is denoted as HEX1

- Note 1: decoderawex HEX1 can check the exchange 

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

If cancomplete is true, it means that the transaction is convertible.

- Node 1: Create lock transation output for 1000 AAA

```bash
	prelockfrom addr1 '{"AAA":1000, "":0.02}'
  {
      "txid" : "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
      "vout" : 0
  }
```

Get txid and vout

- Node 1: Add to exchange transaction and confirm acceptance of 500 BBB exchange

```bash
	addrawex HEX1 txid 0 '{"BBB":500}'

	{
        "hex" : HEX2,
        "complete" : true
    }
```

Output longer hexadecimal text and denote as HEX2. complete:true. That means that the transaction is fully signed and ready for broadcast and confirmation.

- Broadcast transaction

```bash
	sendrawdeal HEX2
```

Return to TXID, wait for the blocking, and inquire the balance to see whether the exchange is successful.

### 5.3.2 Cancel the exchange

How to cancel the exchange before acceptance and broadcasting.

```bash
	disrawdeal HEX1
```

At that time, decode rawex toreturn an error, indicating that the exchange has been successfully canceled.

## 5.4 Multi-signature account transactions and samples

### 5.4.1 Introduction to multi-signature account

Public key encryption, also known as asymmetric cryptography, is a key technology of block chain. Participants in the chain generate their own private key and public address pairs. They keep private keys secret, but they are free to assign relevant addresses. Block chain transactions that operate on a particular address (such as spending its money) must be signed with the corresponding private key. All participants in the chain can then verify these signatures by using a public address without having to look at each other's private keys. Condominium contract account is also known as multiple signatures. The address and transaction broaden the model by creating identities on a chain managed by many parties. CXC use "m/n", where multiple signature addresses are defined as n regular addresses given. For the private key corresponding to at least m addresses, transactions must be simultaneously signed to execute the transactions.

### 5.4.2 Create co-managed contract account address

- Look for the address and find addr1 and addr2
- Create a co-managed account. Node 1 runs the following command to create the 2/2 multiple address and add it to the node's wallet:

```bash
	addmultiaddr 2 '["A1-publickey", "A2-publickey"]'
```

Get the multi-signature address MA1A2, and automatically start tracking. Node 2 can repeat the command of Node 1

```bash
	addmultiaddr 2 '["A1-publickey", "A2-publickey"]'
```

### 5.4.3 Make transaction through multi-signature account

Send funds through a multi-signature account. Since this is a 2/2 co-managed contract account, the process will require the co-signature of both addresses.

- Node 1: Create the transaction and partially 

```bash
	setuprawsendfrom MA1A2 '{"XXXXXXXXXXXXXXXXXXXX ":{"AAA":500}}' '[]' sign
  {
      "hex" : "XXXXXXXXXXXXXXXXXXXXXXXX",
      "complete" : false
  }
```

Respond to complete:false. The hexadecimal hex field is HEX1. The partial signature has been made.

- Signature of Node 2:

```bash
	  signrawdeal HEX1
  
	  {
		  "hex" : "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
		  "complete" : true
	  }
```

Respond tocomplete:true， The hexadecimal hex field is HEX2.This means that the transaction is signed and can be broadcast to the block chain.

- Broadcast the transaction

```bash
	sendrawdeal HEX2
```

## 5.5ChainDB

ChainDB provides a complete solution for both information system and Dapp that focuses on general data retrieval, timestamps and archiving, rather than asset transfers between participants. ChainDB is understood as a table in a database or a field in a table.CXC currently supports the performance of handling capacity of 2M per transaction and 16M per block (up to 64M). Distributed fragmented ChainDB is supported thought the design of out-of-chain application block, which supports integrated up mixed storage on and off the line.

### 5.5.1ChainDB

Relative to the design of traditional information system development, the table structure and field are used to create ChainDB. All operations of ChainDB of the creator are permitted by default. True represents that anyone can write. On Note 1

```bash
	setupdatamod DM1 true
```

### 5.5.ChainDB writes data

Write database relative to the traditional information system development. On Note 1 

```bash
	#hex type of data
	senditem DM1 key01 646174616d6f64
	#text data
	senditem DM1 key02 '{"text":"Hello World"}'
	#json data
	senditem DM1 key03 '{"json":{"name":"name1","age":20}}'
```

### 5.5.ChainDB retrieves data

Check data from databased relative to the traditional information system development. On Note 2 ChainDB:

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

The data storage is based on the principle that whoever uses should store. Although the all-node wallet stores all the blocks, but if it is not subscribed relevant data storage is not structured and data is not stored. After subscription, relevant data is structured and related out-of-chain application block is synchronized. The pointer points to the application block content in the left rotating chain, which avoids wasting unnecessary disk space. Data content of ChainDB of Node 2

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

are many ways to retrieve the content of data items, including ChainDB, sender of data items, keyword, timestamp, etc. CXCChain adopts a structured data storage method for data storage, so the retrieval efficiency is extremely high.

```bash
	# Show the key index for ChainDB
	showdatakeys DM1 
	# Show items for which key01 is the keyword
    showdatakeyitems DM1 key01 
	# Show data item publisher information in ChainDB
    showdatasenders DM1
	# Show the specific item published by the ADDRESS publisher in ChainDB
    showdatasenderitems DM1 ADDRESS 
```

### 5.5ChainDB advanced usage-JSON merge

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

## 5.6 Out-of-chain data storage

- Node 1:  First, write the data stored by very large data (out-of-chain application blocks) into the cache

```bash
	setupcache
```

- Get the cache value and call it cache1

- Add the file to the cache

```bash
	addcache cache1 file /Users/root/test.jpg
```

Return file size:956303

- Get the cache value and record as ey04

```bash
	senditem DM1 key04 '{"cache":cache1}' outchain
```

Return to TXID

- Node 2: Query data 

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

- We can see the new items listed. If data is"available" : false  it indicates that the application block outside the chain has not been completed synchronously. We can query the synchronization queue

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

- Once the block queue is listed as empty,the item's data should be available and its contents can be checked hexadecimally:

```bash
	showtxoutdata TXID 0 512
```

# 6.Scenario intelligent contract reference

## 6.1 Intelligent contract reference for token financing

Main objective:To set automated atomic transactions and realize the underlying technical framework of intelligent contracts for token financing. Scenario description: Create an asset to be financed (token code is CCC, minimum unit is 0.01, total amount is 10,000,000, amount of financing is 5,000,000, exchange is 500cxc (existing assets or native currency in the system))

- Create a new address on the node server

```bash
	#Get
	addnewaddr 
```

- Issue10,000,000 CCC to A2 from the management address A1 with the minimum unit of 0.01. It cannot be added again.

```bash
	sellfrom A1 A2 '{"name":"CCC","open":false}' 10000000 0.01 0.01 '{"Build":"CN", "Index":"01", "for":"OfferCoin"}'
	#Return to TXID
	#Check the issuance of new assets
	showassets CCC
```

- Initiate the atomic transaction contract of token financing .

```bash
	  #Now let's create a new transaction.

	  prelockfrom A2 '{"CCC":5000000}'

	  #Get txid:TXID1
	  #Get vout:VOUT1

	  #Set up the atomic transaction, specify that we use 500 CXC (underlying code of ""), and exchange 5,000,000 CCC:

	  setuprawex TXID1 VOUT1 '{"":500}'

	  #Output HEX1, a hexadecimal block, and then send this HEX1 through ChainDB to all nodes automatically, or to participants offline. 
```

- Participant 1 participates in the exchange

```bash
	  prelockfrom 1-address '{"":100}'

	  #Get txid:TXID2
	  #Get vout:VOUT2

	   #Now we add the 100 CXC  offer into the transaction and require to exchange 1,000,000CCC (according to the proportion of the original offer):

	  addrawex HEX1 TXID2 VOUT2 '{"CCC":1000000}'

	  #Output hexadecimal HEX2， complete : false. That means that the exchange is not balanced. See which assets are still being offered and requested in the transaction:

	  decoderawex HEX2

	 #This should indicate the offer of 4,000,000 CCC and the requirements of 400 CXC. For more details:

	  decoderawex HEX2 true

	 #The exchanges field shows the exchange transaction of all individual offer and ask stages. By now, the address beside participates. 
```

- Now Participant 2 continues to participate in the exchange:

```bash
	  prelockfrom 2-address '{"":200}'

	  #Get txid:TXID3
	  #Get vout:VOUT3

	  addrawex HEX2 TXID3 VOUT3 '{"CCC":2000000}' 

	  #Output the longest hexadecimal HEX3
```

- Participant 3 continues to participate in the exchange

```bash
	  prelockfrom 3-address '{"":200}'

	  #Get txid:TXID4
	  #Get vout:VOUT4

	  completerawex HEX3 TXID4 VOUT4 '{"CCC":2000000}' 4322074fdf6872d65652077617973

	  #Output hexadecimal HEX4. We can broadcast and confirm:

	  sendrawdeal HEX4

	#Thus, the whole token financing process is completed.  
```

