# Third-party system docking guide

This document is for third-party APP to call up CXC wallet for authorized login and payment.

CXC wallet's Scheme is cxcwallet

## Open access

The third-party app needs to provide the following data in order to open the access service.

- Access system name

- Access system icon URL

- Callback secure domain name 1

- Callback secure domain name 2

- Callback secure domain name 3

- Callback APPScheme

  Note: The callback secure domain name can be provided with at least one protocol and domain name, and the system matches the callback request for any port that contains the domain name and protocol. If https://www.bitcoin.com is available, the system can call back https://www.bitcoin.com https://www.bitcoin.com:8080 but cannot call back http://oauth.bitcoin.com or Callback http://www.bitcoin.com.
  
  If the APP callback is HTTP/HTTPS, just fill in the secure domain name.

After the third-party application is applied, appUuid, securityId, rsaPrivatekey, rsaPublickey will be obtained. The role will be introduced below.

Click on the "Eyes" symbol in the DAPP access request record. If you have any questions, please send an email to cxcdapp@outlook.com or leave a message on github.

The authorization process is divided into four steps:

1. Guide the user to enter the authorization page to approve the authorization. The CID user clicks to confirm and obtain the code.

2. The wallet APP returns the code to the access party.

3. The access party uses the code and securityId to request the wallet to obtain the user cid and address.

## APP authorized login

### 	1, APP calls the wallet to apply for authorization to log in

​	 The third-party APP uses the SchemeURL to pick up the wallet app and uses the URL-encoded JSON to pass the following parameters.

| KEY   | description               |
| ----- | ----------------------------- |
| type  | input "login" required |
| appid | AppUuid obtained when opening the access service is required |
| callback| Callback address in schemeurl format. Required |
| callbackType | When 0, the return code will be stitched by "?" or "&". For 1 time, code is directly spliced at the end of the callback. Default 0 is not required. |
| openCallBackType | 0 or empty, the wallet jumps to the callback application. <br /> When 1, the wallet calls the callbackurl and minimizes the wallet. <br /> not required |

Before the overall URL encoding

```
cxcwallet://{"type":"login","appid":"121212-121212-121212","callback":"appscheme://a/b/c?d=1&redirect=http%3A%2F%2Fwww.baidu.com%2Fs%3Fwd%3D12345"}
```

After the overall URL is encoded

```bash
cxcwallet://%7B%22type%22%3A%22login%22%2C%22appid%22%3A%22121212-121212-121212%22%2C%22callback%22%3A%22appscheme%3A%2F%2Fa%2Fb%2Fc%3Fd%3D1%26redirect%3Dhttp%253A%252F%252Fwww.baidu.com%252Fs%253Fwd%253D12345%22%7D
```

### 2, get CID and Address

After the user allows authorization in the CXC wallet, the wallet generates an authorization code and carries the code parameter to jump to the address of the callback callback.

> Code is valid for 1 minute and expires after one use.

After getting the code, request the following link to get cid and address

 https://mobile.cxcblock.com:5700/future/oauth/login/cid?code={CODE}&securityId={securityId}

| parameter name | description                                              |
| -------------- | -------------------------------------------------------- |
| code           | Fill in the code obtained in the first step              |
| securityId     | SecurityId obtained when third-party access is activated |

Return instructions

The JSON packet returned when correct is as follows

```json
{
    "result":{
    	"cid":"cid",
    	"address":"address"
    },
    "success":true,
    "errorCode":"",
    "errorMsg":""
 }
```

**The error return code errorCode is described as follows:**

| error code         | description         |
| ------------------ | ------------------- |
| -OAuthCheckLogin-1 | securityId error    |
| -OAuthCheckLogin-2 | Code does not exist |
| -OAuthCheckLogin-3 | Query failed        |

## WEB scan code authorized login

### 1、Create a QR code

The accessor needs to provide the URL QR code to supply the CXC wallet scan code login. URL parameters are as follows

| parameter name | description |Required or not|
| -------- | --------- |---------|
| type     | Fill in the login |Yes|
| platform | Fill in the web |Yes|
| callback | Callback address brought into the code after authorization |Yes|
| appid | Accessor's appuuid |Yes|
| callbackType | When 0, the return code and token will be spliced by "?" or "&". For 1 time code and token are directly spliced at the end of the callback. Default 0 |no|

example：

```shell
cxclogin://?type=login&platform=web&callback=http%3A%2F%2F192.168.1.96%3A3000%2Fa%3Fc%3D1&appid=sss
```

After the user confirms the authorization, the wallet APP will bring the authorization code and the token in the QR code back to the callback parameter address.

> Code is valid for 1 minute and expires after one use.

### 2、Get CID and Address

After getting the code, request the following link to get cid and address

 https://mobile.cxcblock.com:5700/future/oauth/login/cid?code={CODE}&securityId={securityId}

| parameter name | description                                              |
| -------------- | -------------------------------------------------------- |
| code           | Fill in the code obtained in the first step              |
| securityId     | SecurityId obtained when third-party access is activated |

Return instructions

The JSON packet returned when correct is as follows

```json
{
    "result":{
    	"cid":"cid",
    	"address":"address"
    },
    "success":true,
    "errorCode":"",
    "errorMsg":""
 }
```

**The error return code errorCode is as follows：**

| error code         | description         |
| ------------------ | ------------------- |
| -OAuthCheckLogin-1 | securityId error    |
| -OAuthCheckLogin-2 | Code does not exist |
| -OAuthCheckLogin-3 | Query failed        |



## Wallet payment

The third-party APP can call CXC to complete the process of completing the order payment.

### 1、Apply for user payment

#### 1.1APP call

The APP wallet picks up the CXC wallet via the schemeurl and passes the following parameters via the URL-encoded JSON object.

| parameter name   | description                                                  |
| ---------------- | ------------------------------------------------------------ |
| type             | Types of. Fill in pay                                        |
| uuid             | The appuuid obtained at the time of opening must be.         |
| orderId          | Third party business system order number, must.              |
| toAddr           | Collection address, must.                                    |
| assetType        | Type of payment asset, must.                                 |
| amount           | The amount of the payment must be.                           |
| callbackUrl      | The payment succeeds cxc service informs the background callback of the payment status that the host needs to be within the secure domain name and must be. |
| callbackScheme   | The appschemeurl that is returned after the payment is completed must be. |
| desc             | The description will be presented to the user.               |
| rsaContent       | Signature data. The parameter must be signed using the rsa private key obtained during access. |
| openCallBackType | APP callback method after payment is completed. When 0, the phone opens the callbackScheme. If the other APP is not opened at 1 time, only the wallet APP is minimized, and this operation does not affect the payment status callback. |

example:

Before URL encoding

```
cxcwallet://{"type":"pay","rsaContent":"XXXXXXXXXXXXX","uuid":"sss","orderId":"121212","toAddr":"XXXXXXXXXXXXXXXXX","assetType":"CXC","amount":"0.001","callbackUrl":"http://localhost/pay?u=http%3A%2F%2Fwww.baidu.com%2Fs%3Fwd%3D12345","desc":"hhh"}
```

> Note: amount is a string type

After URL encoding

```
cxcwallet://%7B%22type%22%3A%22pay%22%2C%22rsaContent%22%3A%22XXXXXXXXXXXXX%22%2C%22uuid%22%3A%22sss%22%2C%22orderId%22%3A%22121212%22%2C%22toAddr%22%3A%22XXXXXXXXXXXXXXXXX%22%2C%22assetType%22%3A%22CXC%22%2C%22amount%22%3A%220.001%22%2C%22callbackUrl%22%3A%22http%3A%2F%2Flocalhost%2Fpay%3Fu%3Dhttp%253A%252F%252Fwww.baidu.com%252Fs%253Fwd%253D12345%22%2C%22desc%22%3A%22hhh%22%7D
```

#### 1.2 two-dimensional code call

The access party needs to construct a URL QR code to scan the code by the wallet APP. The parameter uses the get method, the content is as follows

| parameter name | description                                                  |
| -------------- | ------------------------------------------------------------ |
| type           | Fill in pay                                                  |
| rsaContent     | Signature data. The parameter must be signed using the rsa private key obtained during access. |
| uuid           | The appuuid obtained at the time of opening must be.         |
| orderId        | Third party business system order number, must.              |
| toAddr         | Collection address, must.                                    |
| assetType      | Type of payment asset, must.                                 |
| amount         | The amount of the payment must be.                           |
| callbackUrl    | The payment succeeds cxc service informs the background callback of the payment status that the host needs to be within the secure domain name and must be. |
| desc           | The description will be presented to the user.               |

example:

```
cxcpay://?type=pay&rsaContent=CTXXXXXXXXXXXLwGTq%2BJr766pX4pw%3D%3D&uuid=sss&orderId=121212&toAddr=1xxxxxxxxxxxxxx&assetType=CXC&amount=0.001&callback=http%3A%2F%2F192.168.1.96%3A3000%2Fa%3Fc%3D1&desc=aasdasdas
```



#### Signature algorithm

Use the rsaPrivatekey at the time of application to sign the mount, assetType, callbackUrl, orderId, toAddr, uuid parameters using the SHA1WithRSA algorithm.

Suppose the parameters to be signed are as follows:

```json
{"uuid":"uuidVal","orderId":"orderIdVal","toAddr":"toAddrVal","assetType":"assetTypeVal","amount":"amountVal","callbackUrl":"callbackUrlVal"}
```

##### 1. Arrange the parameters into a json structure in the order of amount, assetType, callbackUrl, orderId, toAddr, uuid.

##### 2. Use SHA1withRSA to sign the parameters. The signed content is BASE64 encoded as rsaContent.

### 2、After the payment is completed, jump back to the original APP and notify the payment status in the background.

If the payment is successful and the block is confirmed, a GET form request will be sent to the callbackUrl.

The parameters carried in the CallbackUrl are as follows:

| parameter name | description                                                  |
| -------------- | ------------------------------------------------------------ |
| uuid           | AppUuid applied when the payment service is opened           |
| orderId        | Third-party business system order number                     |
| fromAddr       | Payment address                                              |
| toAddr         | Collection address                                           |
| assetType      | Payment asset type                                           |
| amount         | Payment amount, string type                                  |
| txid           | The chain transaction number after successful payment        |
| callbackUrl    | Payment successful cxc service notification payment status background callback path |
| rsaContent     | Signature data, sign the parameters using the rsa private key obtained during access |


### Signature algorithm

The rsaPrivatekey at the time of application of the APP uses the SHA1WithRSA algorithm to sign the parameters.

##### 1. Arrange the parameters into a json structure in the order of amount, assetType, callbackUrl, fromAddr, orderId, toAddr, txid, uuid.

> Amount is a string type, directly into JSON, do not turn to floating point string (1.00000)

##### 2. The parameter is signed by SHA1withRSA, and the signed content is BASE64 encoded as rsaContent. The third-party dapp can use the public key to verify the parameters and the signed information.

The information that the third-party Dapp needs to return is as follows:

| Return field | description                                                  |
| ------------ | ------------------------------------------------------------ |
| success      | True or false, if true, the callback succeeds, if false, the callback fails |
| errorCode    | Error code, if success is true, it can not be returned.      |
| errorMsg     | Error message, if success is true, it can't be returned.     |



## Current price inquiry

interface address

```
[GET] https://mobile.cxcblock.com:5700/future/oauth/pay/price?currency=USD&securityId=000000
```

Frequency limit: 10 times per minute

Interface parameter

| parameter name | Parameter Description                                        | Required or not |
| -------------- | ------------------------------------------------------------ | --------------- |
| currency       | Currency type. Currently only supports CNY, USD, EUR, GBP, JPY, KRW | Yes             |
| securityId     | Access party securityId                                      | Yes             |

Return result

| name      | description                                                  |
| --------- | ------------------------------------------------------------ |
| success   | True or false, if true, the call succeeds, if false, the call is incorrect |
| errorCode | error code. Valid when success is false                      |
| errorMsg  | Error message. Valid when success is false                   |
| result    | Get the current price of the specified currency when success is true, otherwise return empty |

error code

| error code   | description         |
| ------------ | ------------------- |
| -pay-price-1 | SecurityId error    |
| -pay-price-2 | currencytype error  |
| -pay-price-3 | Acquisition failure |
