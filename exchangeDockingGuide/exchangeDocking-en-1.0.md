# Exchange docking guidelines

The main purpose of this paper is to assist the exchange on-line CXC transactions.

## Basic concept

The UTXO model is utilized to CXC account system, so the on-line CXC is similar to BTC.

## Linux node deployment

### install

```bash
download https://github.com/cxcblock/cxcs/archive/master.zip
sudo chmod +x install.sh
./install.sh
```

### start

```bash
./cxcsz CXCChain
```

#### CLI

```bash
./cxcsi CXCChain command
```

#### stop

```bash
./cxcsi CXCChain stop
```

#### Connect to the specified node at startup

```bash
./cxcsz CXCChain@ip:port
```

## Mac node deployment

### install

```bash
download https://github.com/cxcblock/cxcs/archive/master.zip
sudo chmod +x install.sh
./install.sh
```

### start

```bash
./cxcsz CXCChain
```

#### CLI

```bash
./cxcsi CXCChain command
```

#### stop

```bash
./cxcsi CXCChain stop
```

## Windows node deployment

### start

```bash
cxcsz.exe CXCChain
```



#### CLI

```bash
cxcsi.exe CXCChain command
```

#### stop

```bash
cxcsi.exe CXCChain stop
```

#### Connect to the specified node at startup

```bash
cxcsz.exe CXCChain@ip:port
```

## Node startup important parameters

Storage directory

```bash
-datadir=/data/.cxcs
```

Blockchain port

```bash
-port=7519
```

Rpc port

```bash
-rpcport=7518
```

Allow ip

```bash
-rpcallowip=127.0.0.1

```

Each time a new block is generated, the specified shell command is triggered, where %s is the HASH of the new block.

```bash
-blocknotify='curl http://192.168.1.10:8080/cxc?blockHash=%s'

```

E.g

```bash
./cxcsz CXCChain -datadir=/data/.cxcs -port=7319 -rpcport=7318 -blocknotify=‘curl http://192.168.1.10:8080/cxc?blockHash=%s'

```

After the chain is started, the startup parameters can be written to the conf configuration file. The configuration file is located at /data/.cxcs/CXCChain/cxcs.conf.

E.g

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

The node needs to be restarted after setting.

```bash
showinfo

```

result

```json
{
    "paytxfee" : "0.0001",
    "rpcport" : 7318,
    "relayfee" : "0.0001",
    "maxout" : 10000000000000000
}

```

Where paytxfee is the base fee setting of the current node, and rpcport is the callable rpc port.

The minimum fee must be set by settxfee, otherwise it may result in insufficient priority! !

```bash
settxfee 0.0001
```

Call showinfo to view the information.

## How to call a command via rpc

1. Confirm that the caller IP has been assigned to allowip.

2. View the account password in cxcs.conf in the blockchain directory.

3. Initiating an HTTP request to the node RPC

   ```bash
   $ curl --user cxcsrpc:Ch6iaD7aPqegYDenzkDz3ttFCaBUTzBeqsmgye5Mn98o --data-binary '{"jsonrpc": "2.0", "id":"rpccall", "method": "setupkeypairs", "configs": [2] }' -H 'content-type: text/plain;' http://127.0.0.1:7318
    
    {"result":[{"address":"address1","pubkey":"pubkey1","privkey":"pribkey1"},{"address":"address2","pubkey":"pubkey2","privkey":"privkey2"}],"error":null,"id":"rpccall"}
   
   ```

   

## Generate recharge address

There are  two methods to generate recharge address:

- Method I：Dynamically create a CXCChain address.

  Call the following command：

  ```bash
  ./cxcsi CXCChain addnewaddr
  ```
  
. /cxcsi CXCChain will be hidden later.
  
The created address will be imported directly into the wallet file.
  
- Method II：Create wallet address in batches in advance. 

  Call the following command：

  ```bash
  setupkeypairs (count)
  ```
  
where,the parameter count is the number of public/private key pairs, and the default is 1. Note that the address created in this way needs to be imported into the wallet through the importaddr or importprivkey command to directly call the transfer command. For example：
  
- setupkeypairs 100 
  - importaddr '["ADDR1","ADDR2","ADDR3"]' false
  
## Check the validity of the address
  
 Check the validity of the account address by calling the following command:
  
```bash
  validaddr (address)
  ```
  
    The returned results are as follows:

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

If the isvalid field is false, it is an invalid address.

## Monitor user recharge

To monitor user recharge, first check the block synchronization status and analyse the block information so as to confirm whether the user recharge.

### Check block synchronization

   call the following command：

```bash
  showchain
```

   The returned result is as follows：

```json
  {
    "blocks" : 13169,
      "headers" : 14009,
      "bestblockhash" : "00c44114d67728e2ea1ba1c323aa6e4d8ef18d8ee178a6d712634c775fc56614"
  }
```

   Where headers are the number of block headers,Blocks are the number of blocks that have been synchronized.

### Analyse the block information

  call the following command：

```bash
  showblock hash|height 4
```

 We use the UTXO model, and the exchange needs to analyse the VOUT part of each TX, where value is the local asset, namely CXC, and asset is the created asset. There are two methods to compare recharge information:

1. The exchange records all transactions related to itself as a record of recharge and withdrawal by users. If There is an address belonging to the exchange in VOUT, the user balance of the recharge address in the database (and possibly including the issuing assets, of course) is modified.
2. If there is an address belonging to the exchange in VOUT, first keep the recharge record in the database，wait for several blocks to confirm before modifying the user balance.

```bash
  showblock 10006 4
```

  the returned results are as follows：

```json
  {
      "hash" : "XXXXXXXXXXXXXXXXXXXXXXXXX",
      "miner" : "1XXXXXXXXXXXXXXXXXXXXXXXXXX",
      "confirmations" : 7121,
      "size" : 733,
      "height" : 7015,
      "version" : 3,
      "merkleroot" : "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
      "tx" : [
          {
              "txid" : "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
              "version" : 1,
              "locktime" : 0,
              "vin" : [
                  {
                      "coinbase" : "XXXXXXXXXXXXXXX",
                      "sequence" : 4294967295
                  }
              ],
              "vout" : [
                  {
                      "value" : "125.0001",
                      "n" : 0,
                      "scriptPubKey" : {
                          "asm" : "OP_DUP OP_HASH160 a2817351e690e49d93138c890ffec060bc7bc91b OP_EQUALVERIFY OP_CHECKSIG",
                          "hex" : "76a914a2817351e690e49d93138c890ffec060bc7bc91b88ac",
                          "reqSigs" : 1,
                          "type" : "pubkeyhash",
                          "addresses" : [
                              "1FpFYTG5ZcR3tk1TF3rQR4XcytoqSUhgPF"
                          ]
                      }
                  },
                  {
                      "value" : "0.00",
                      "n" : 1,
                      "scriptPubKey" : {
                          "asm" : "OP_RETURN 53504b6246304402207a717402faab3907a127809c6646be4d18ee50602eaa0c513d46321f6b6a2b44022054b12584afd5bcc65c9ee58c84ffe3cf04bfaf81ad40fb07a1cc264ef383990d0221033fdb7e03f6d30013fa75702c52c7590f4bf03edf90c94319d6e7487d12a40e27",
                          "hex" : "6a4c6e53504b6246304402207a717402faab3907a127809c6646be4d18ee50602eaa0c513d46321f6b6a2b44022054b12584afd5bcc65c9ee58c84ffe3cf04bfaf81ad40fb07a1cc264ef383990d0221033fdb7e03f6d30013fa75702c52c7590f4bf03edf90c94319d6e7487d12a40e27",
                          "type" : "nulldata"
                      },
                      "data" : [
                          "XXXXXXXXXXXXXXXXXXXXXXXXXX"
                      ]
                  }
              ],
              "hex" : "XXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
          },
          {
              "txid" : "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
              "version" : 1,
              "locktime" : 0,
              "vin" : [
                  {
                      "txid" : "XXXXXXXXXXXXXXXXXXXXXXXXXXXX",
                      "vout" : 1,
                      "scriptSig" : {
                          "asm" : "3045022100a9ca7df6bd338ae944e654edccb89eb2d3b8e8e3803b67719165d638ab2836a7022076bd9a24f2a4649a13c6e2871ca96bae0411700ed73a816458c0e8c8b0c8ea6b01 02caa0ea1c20838e359db6a4300a25e3e10f6e05fceb3f6c78a635d770e7e5ae1a",
                          "hex" : "483045022100a9ca7df6bd338ae944e654edccb89eb2d3b8e8e3803b67719165d638ab2836a7022076bd9a24f2a4649a13c6e2871ca96bae0411700ed73a816458c0e8c8b0c8ea6b012102caa0ea1c20838e359db6a4300a25e3e10f6e05fceb3f6c78a635d770e7e5ae1a"
                      },
                      "sequence" : 4294967295
                  },
                  {
                      "txid" : "XXXXXXXXXXXXXXXXXXXXXXXXXX",
                      "vout" : 0,
                      "scriptSig" : {
                          "asm" : "304402207d77cb750d5cbda6b3e7ab38db648e79c2e4b6b3965e73efb39bddc52a5608e002200a86c13bc51834b41d54ab967e12f890b9d021a73bf4607aae5f90232e31931801 02caa0ea1c20838e359db6a4300a25e3e10f6e05fceb3f6c78a635d770e7e5ae1a",
                          "hex" : "47304402207d77cb750d5cbda6b3e7ab38db648e79c2e4b6b3965e73efb39bddc52a5608e002200a86c13bc51834b41d54ab967e12f890b9d021a73bf4607aae5f90232e319318012102caa0ea1c20838e359db6a4300a25e3e10f6e05fceb3f6c78a635d770e7e5ae1a"
                      },
                      "sequence" : 4294967295
                  }
              ],
              "vout" : [
                  {
                      "value" : "0.00",
                      "n" : 0,
                      "scriptPubKey" : {
                          "asm" : "OP_DUP OP_HASH160 1612e2c2dd8c0b6b48ad90fe76f59068261272c2 OP_EQUALVERIFY OP_CHECKSIG 73706b7194c211082f23d797e59f5b8d561f8987a086010000000000 OP_DROP",
                          "hex" : "76a9141612e2c2dd8c0b6b48ad90fe76f59068261272c288ac1c73706b7194c211082f23d797e59f5b8d561f8987a08601000000000075",
                          "reqSigs" : 1,
                          "type" : "pubkeyhash",
                          "addresses" : [
                              "131iVRoiGg6PRbbCP1N1a2PQBGyDbbiRtb"
                          ]
                      },
                      "assets" : [
                          {
                              "name" : "BTC",
                              "selltxid" : "87891f568d5b9fe597d7232f0811c29415a9470a87b532e088d4c60693c41d31",
                              "assetref" : "990-301-35207",
                              "qty" : 0.1,
                              "raw" : 100000,
                              "type" : "transfer"
                          }
                      ]
                  },
                  {
                      "value" : "30102.5095",
                      "n" : 1,
                      "scriptPubKey" : {
                          "asm" : "OP_DUP OP_HASH160 bedfbb65d158bed92803dd2c217996f567717a11 OP_EQUALVERIFY OP_CHECKSIG 73706b7194c211082f23d797e59f5b8d561f8987e0a57e0000000000 OP_DROP",
                          "hex" : "76a914bedfbb65d158bed92803dd2c217996f567717a1188ac1c73706b7194c211082f23d797e59f5b8d561f8987e0a57e000000000075",
                          "reqSigs" : 1,
                          "type" : "pubkeyhash",
                          "addresses" : [
                              "1JQFQd2nGBVLVtVq4HxNrD3rT4ws6XXcVi"
                        ]
                      },
                    "assets" : [
                          {
                            "name" : "BTC",
                              "selltxid" : "87891f568d5b9fe597d7232f0811c29415a9470a87b532e088d4c60693c41d31",
                            "assetref" : "990-301-35207",
                              "qty" : 8.3,
                              "raw" : 8300000,
                            "type" : "transfer"
                          }
                      ]
                  }
              ],
              "hex" : "XXXXXXXXXXXXXXXXXXXXXXXXXX"
          }
      ],
      "time" : XXXXXXXXXXXXXX,
      "nonce" : 351,
      "bits" : "2000ffff",
      "difficulty" : 5.96046447753906e-8,
      "chainwork" : "00000000000000000000000000000000000000000000000000000000001b6800",
      "prevblockhash" : "002a3322125c41fd2865ee4972d3f59083c048c049ca50a59aba2b325d9eaf1e",
      "nextblockhash" : "000bbf856cfd829bdc665ad09d42ba0d4e4e59ad54411468247c229385077631"
  }

```

## Handling withdrawal requests

1. Record user withdrawal request, and change balance；
2. Call the send or sendfrom command to send the transaction to the user's withdrawal address. For details, please refer to the Developer Documentation.
3. After the command is successfully executed, txid will be returned and recorded in the database;
4. After the block is confirmed, the withdrawal is successful.

## Offline signature transaction

### uspent output

```bash
showunspent ( minconf maxconf addresses )
```

> Configs

| Configs   | Description                                         |
| --------- | --------------------------------------------------- |
| minconf   | The minimum confirmations to filter                 |
| maxconf   | Default=9999999 The maximum confirmations to filter |
| addresses | A json array of addresses to filter                 |

> e.g.

```bash
	showunspent 6 9999999 "[\"address1\",\"address2\"]"
```

> result

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

### Assembly transaction

  1.The exchange obtains the address by unresolving the unspent by parsing the block. The calling command is as follows:

```bash
	setuprawdeal [{"txid":"id","vout":n},...] {"address":amount,...} ( [data] "action" )
```

> configs

| Configs   | Description                                                  |
| --------- | ------------------------------------------------------------ |
| deals     | deals                                                        |
| addresses | Object with addresses as keys, see help addr-all for details. |
| data      | Array of hexadecimal strings or data objects, see help all-data for details. |
| action    | Default "" Additional actions: "lock", "sign", "lock,sign", "sign,lock", "send". |

> Result

```bash
	"deal"                                                   Hex string of the deal (if action= "" or "lock")
  or
{                                                        A json object (if action= "sign" or "lock,sign" or "sign,lock")
  "hex": "value",                                        The raw deal with signature(s) (hex-encoded string)
  "complete": true|false                                 If deal has a complete set of signature (0 if not)
}
  or
"hex"                                                    The deal hash in hex (if action= "send")

```
Pay attention to the vin of the fee,

The number of bytes is calculated as follows:

10+vin * 180+vout * 34

If vin contains issued assets

+10+( Number of types of assets issued )* 24

If you include metadata (data)

+11+ metadata bytes

Handling fee = number of bytes * 0.0000001

2.Set the change address and specify the transaction fee through addrawchange

> Command

```bash
	addrawchange "tx-hex" "address" ( fee )
```

> configs

| Configs | Description    |
| ------- | -------------- |
| tx-hex  | Hex            |
| address | change address |
| fee     | Fee            |

> e.g.

```bash
	addrawchange "HEX""ADDR"
	addrawchange "HEX""ADDR" 0.01
```

> Result

```bash
	hex
```

The default change address is the originating transfer address.

The last one in vout is the change record

It is recommended that the exchange's change address be set separately to prevent resolving vout to zero.

3.Offline private key signing transaction via signrawdeal

> command

```bash
	signrawdeal "tx-hex" ( [{"txid":"id","vout":n,"scriptPubKey":"hex","redeemScript":"hex"},...] ["privatekey1",...] sighashtype )

```

> Configs

| Configs     | description                                                  |
| ----------- | ------------------------------------------------------------ |
| Tx-hex      | Hex                                                          |
| prevtxs     | An json array of previous dependent deal outputs             |
| privatekeys | A json array of base58-encoded private keys for signing      |
| sighashtype | Default=ALL The signature hash type."ALL","NONE","SINGLE","ALL\|ANYONECANPAY","NONE\|ANYONECANPAY","SIGLE\|AYONECANPAY" |

> result

```bash
	{
	  "hex": "value",                           The raw deal with signature(s) (hex-encoded string)
	  "complete": true|false                    If deal has a complete set of signature (0 if not)
	}
```

4.sendrawdeal

> Command

```bash
	sendrawdeal "tx-hex" ( allowhighfees )

```

> Configs

| Configs       | Description                    |
| ------------- | ------------------------------ |
| Tx-hex        | The hex string of the raw deal |
| allowhighfees | Default=false, Allow high fees |

> e.g.

```bash
	sendrawdeal "signedhex"
```

> Result

```bash
	txid
```

例：

```shell
	setuprawdeal '[{"txid":"526be568b3756124701e7d8c639dd3ffba1a40947cc2573fada996c1e08b4c89","vout":0},{"txid":"526be568b3756124701e7d8c639dd3ffba1a40947cc2573fada996c1e08b4c89","vout":1}]'  '{"12c6DSiU4Rq3P4ZxziKxzrL5LmMBrzjrJX":{"":30}}'
	#result
0100000002894c8be0c196a9ad3f57c27c94401abaffd39d638c7d1e70246175b368e56b520000000000ffffffff894c8be0c196a9ad3f57c27c94401abaffd39d638c7d1e70246175b368e56b520100000000ffffffff0180c3c901000000004f76a914119b098e2e980a229e139a9ed01a469e518e6f2688ac3473706b71ef28a6c20ae4fd378fbd6eed144bfcff80969800000000000e64ad8c6ebffc4d749dbd3e1f93090f002d3101000000007500000000

```

### Addrawchange

```shell
addrawchange 0100000002894c8be0c196a9ad3f57c27c94401abaffd39d638c7d1e70246175b368e56b520000000000ffffffff894c8be0c196a9ad3f57c27c94401abaffd39d638c7d1e70246175b368e56b520100000000ffffffff0180c3c901000000004f76a914119b098e2e980a229e139a9ed01a469e518e6f2688ac3473706b71ef28a6c20ae4fd378fbd6eed144bfcff80969800000000000e64ad8c6ebffc4d749dbd3e1f93090f002d3101000000007500000000 12c6DSiU4Rq3P4ZxziKxzrL5LmMBrzjrJX 0.001

```

### Signrawdeal and sendrawdeal

```shell
signrawdeal dealhex2 '[]' '["privatekey1","privatekey2"]'
sendrawdeal dealhex2 

```

```json
{
    "hex" : "xxxxxxx",
    "complete" : true
}

```



## Related commands

### Check balances

  Use showallbals or showaddrbals to check user balances,The minimum unit of assets is 6.

### Query transaction information

#### showdeal

> description

Returns the specified wallet transaction details. This command can only query related transactions at the address of this node.

> command

```bash
	showdeal txid
```

> parameter 

| parameter | description |
| --------- | ----------- |
| txid      | txid        |

> e.g.

```bash
	showdeal  "txid"

```

> result

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

> description

Query transaction information。

> command

```bash
	showrawdeal "txid" 

```

> parameter

| parameter | description |
| --------- | ----------- |
| txid      | txid        |

> e.g.

```bash
	showrawdeal "txid"

```

> returned value

```bash
	hex Complete hex information for the transaction

```

####  

### Set node service fee

 The basic fee for node transfer accounts can be set by the method of settxfee, and the specific service fee is charged according to the byte.


### Set node collection

To improve node performance, the current node automatically combines a large number of unused outputs (UTXO) belonging to the same address into one unused output. The default number is 50.

	Depending on the situation, the following collection parameters can be specified when the node is running:

integminin    The minimum number of unused outputs that trigger automatic collection

integmaxin   Maximum number of unused outputs for an aggregated transaction

integinterval   Delay between two automatic collections, in seconds

> e.g.

```bash
	cxcsz CXCChain -integminin=50 -integmaxin=100 -integinterval=100
```

### Node encryption and decryption

For details on node encryption and decryption, please refer to https://github.com/cxcblock/doc/tree/master/developer

