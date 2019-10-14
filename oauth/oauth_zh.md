# 第三方系统对接指南

本文档用于第三方APP调起CXC钱包进行授权登录和支付

CXC钱包的Scheme为cxcwallet

## 开通接入权限

第三方APP需要提供以下数据以便开通接入服务

- 接入系统名称

- 接入系统图标URL

- 回调安全域名1

- 回调安全域名2

- 回调安全域名3

- 回调APPScheme

  注：回调安全域名最少可提供1个且含协议与域名,系统匹配完整包含该域名和协议的任何端口的回调请求。如提供https://www.bitcoin.com，系统可回调https://www.bitcoin.com https://www.bitcoin.com:8080 但不能回调http://oauth.bitcoin.com,也不能回调 http://www.bitcoin.com。
  
  如果APP回调为HTTP/HTTPS协议只需填写安全域名。

第三方APP申请后将获得appUuid,securityId,rsaPrivatekey,rsaPublickey(可在DAPP接入申请记录点击“眼睛”符号进行查看，如有疑问请发送邮件至cxcdapp@outlook.com或在github上留言)，作用将会在下面介绍。

授权流程分为四步：

1、引导用户进入授权页面同意授权，CID用户点击确认后获取code

2、钱包APP将code返回给接入方

3、接入方使用code和securityId请求钱包后台获取用户cid与address

## APP授权登录

### 	1、APP调用钱包申请授权登录 

​	 第三方APP通过SchemeURL调起钱包APP并使用URL编码的JSON传递如下参数

| KEY   | 描述                          |
| ----- | ----------------------------- |
| type  | 传login     必填  |
| appid | 开通接入服务时获得的appUuid 必填 |
| callback| schemeurl格式的回调地址。必填 |
| callbackType | 为0时,返回code将通过"?"或者"&"进行拼接。为1时code直接拼接在callback的最后  默认0 非必填 |
| openCallBackType | 0或空时,  钱包跳转callback应用。<br />为1时，   钱包调用callbackurl并最小化钱包。<br />非必填 |

整体URL编码前

```
cxcwallet://{"type":"login","appid":"121212-121212-121212","callback":"appscheme://a/b/c?d=1&redirect=http%3A%2F%2Fwww.baidu.com%2Fs%3Fwd%3D12345"}
```

整体URL编码后

```bash
cxcwallet://%7B%22type%22%3A%22login%22%2C%22appid%22%3A%22121212-121212-121212%22%2C%22callback%22%3A%22appscheme%3A%2F%2Fa%2Fb%2Fc%3Fd%3D1%26redirect%3Dhttp%253A%252F%252Fwww.baidu.com%252Fs%253Fwd%253D12345%22%7D
```

### 2、获取CID与Address

用户在CXC钱包中允许授权后，钱包生成授权code并携带code参数跳转回调callback的地址

> code有效期为1分钟，且使用一次后失效

获取code后，请求以下链接获取cid与address

 https://mobile.cxcblock.com:5700/future/oauth/login/cid?code={CODE}&securityId={securityId}

| 参数名称   | 描述                             |
| ---------- | -------------------------------- |
| code       | 填写第一步获取的code             |
| securityId | 开通第三方接入时获得的securityId |

返回说明

正确时返回的JSON数据包如下

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

**错误返回码errorCode说明如下：**

| 错误码             | 描述           |
| ------------------ | -------------- |
| -OAuthCheckLogin-1 | securityId错误 |
| -OAuthCheckLogin-2 | code不存在     |
| -OAuthCheckLogin-3 | 查询失败       |

## WEB扫码授权登录

### 1、创建二维码

接入方需要提供URL二维码供应CXC钱包扫码登录。URL参数如下

| 参数名称 | 描述      |是否必填|
| -------- | --------- |---------|
| type     | 填写login |是|
| platform | 填写web     |是|
| callback | 授权后将code带入的回调地址 |是|
| appid | 接入方的appuuid |是|
| callbackType | 为0时,返回code与token将通过"?"或者"&"进行拼接。为1时code与token直接拼接在callback的最后  默认0 |否|

例如：

```shell
cxclogin://?type=login&platform=web&callback=http%3A%2F%2F192.168.1.96%3A3000%2Fa%3Fc%3D1&appid=sss
```

用户确认授权后,钱包APP会将授权code与二维码中的token带回callback参数地址中。

> code有效期为1分钟，且使用一次后失效

### 2、获取CID与Address

获取code后，请求以下链接获取cid与address

 https://mobile.cxcblock.com:5700/future/oauth/login/cid?code={CODE}&securityId={securityId}

| 参数名称   | 描述                             |
| ---------- | -------------------------------- |
| code       | 填写第一步获取的code             |
| securityId | 开通第三方接入时获得的securityId |

返回说明

正确时返回的JSON数据包如下

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

**错误返回码errorCode说明如下：**

| 错误码             | 描述           |
| ------------------ | -------------- |
| -OAuthCheckLogin-1 | securityId错误 |
| -OAuthCheckLogin-2 | code不存在     |
| -OAuthCheckLogin-3 | 查询失败       |



## 钱包支付

第三方APP可以调用CXC支付完成下单支付的流程。

### 1、申请用户支付

#### 1.1APP调用

APP钱包通过schemeurl调起CXC钱包，并通过URL编码的JSON对象传递以下参数

| 参数名称         | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| type             | 类型。填写pay                                                |
| uuid             | 开通时获得的appuuid,必须。                                   |
| orderId          | 第三方业务系统订单号，必须。                                 |
| toAddr           | 收款地址，必须。                                             |
| assetType        | 付款资产类型，必须。                                         |
| amount           | 付款金额，必须。                                             |
| callbackUrl      | 付款成功cxc服务通知付款状态的后台回调，host需要在安全域名内，必须。 |
| callbackScheme   | 支付完成后跳转回的appschemeurl，必须。                       |
| desc             | 描述,将展示给用户。                                          |
| rsaContent       | 签名数据。使用接入时获得的rsa私钥对参数进行签名，必须。      |
| openCallBackType | 支付完成后APP回调方式。为0时手机打开callbackScheme。为1时不打开其他APP，仅将钱包APP最小化，此操作不影响支付状态回调。 |

例：

URL编码前

```
cxcwallet://{"type":"pay","rsaContent":"XXXXXXXXXXXXX","uuid":"sss","orderId":"121212","toAddr":"XXXXXXXXXXXXXXXXX","assetType":"CXC","amount":"0.001","callbackUrl":"http://localhost/pay?u=http%3A%2F%2Fwww.baidu.com%2Fs%3Fwd%3D12345","desc":"hhh"}
```

> 注意：amount为字符串类型

URL编码后

```
cxcwallet://%7B%22type%22%3A%22pay%22%2C%22rsaContent%22%3A%22XXXXXXXXXXXXX%22%2C%22uuid%22%3A%22sss%22%2C%22orderId%22%3A%22121212%22%2C%22toAddr%22%3A%22XXXXXXXXXXXXXXXXX%22%2C%22assetType%22%3A%22CXC%22%2C%22amount%22%3A%220.001%22%2C%22callbackUrl%22%3A%22http%3A%2F%2Flocalhost%2Fpay%3Fu%3Dhttp%253A%252F%252Fwww.baidu.com%252Fs%253Fwd%253D12345%22%2C%22desc%22%3A%22hhh%22%7D
```

#### 1.2二维码调用

接入方需构建URL二维码由钱包APP扫码。参数使用get方式，内容如下

| 参数名称    | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| type        | 填写pay                                                      |
| rsaContent  | 签名数据。使用接入时获得的rsa私钥对参数进行签名，必须。      |
| uuid        | 开通时获得的appuuid,必须。                                   |
| orderId     | 第三方业务系统订单号，必须。                                 |
| toAddr      | 收款地址，必须。                                             |
| assetType   | 付款资产类型，必须。                                         |
| amount      | 付款金额，必须。                                             |
| callbackUrl | 付款成功cxc服务通知付款状态的后台回调，host需要在安全域名内，必须。 |
| desc        | 描述,将展示给用户。                                          |

例如：

```
cxcpay://?type=pay&rsaContent=CTXXXXXXXXXXXLwGTq%2BJr766pX4pw%3D%3D&uuid=sss&orderId=121212&toAddr=1xxxxxxxxxxxxxx&assetType=CXC&amount=0.001&callback=http%3A%2F%2F192.168.1.96%3A3000%2Fa%3Fc%3D1&desc=aasdasdas
```



#### 签名算法

使用申请时的rsaPrivatekey运用SHA1WithRSA算法对amount,assetType,callbackUrl,orderId,toAddr,uuid参数进行签名。

假设需签名的参数如下：

```json
{"uuid":"uuidVal","orderId":"orderIdVal","toAddr":"toAddrVal","assetType":"assetTypeVal","amount":"amountVal","callbackUrl":"callbackUrlVal"}
```

##### 1.按照amount,assetType,callbackUrl,orderId,toAddr,uuid顺序将参数排成json结构。

##### 2.对参数使用SHA1withRSA进行签名，签名后的内容进行BASE64编码即为rsaContent。

### 2、支付完成后跳回原APP，后台通知支付状态

若支付成功，且得到区块确认，那么将会发送GET表单请求至callbackUrl。

CallbackUrl中携带的参数如下：

| 参数名称    | 描述                                            |
| ----------- | ----------------------------------------------- |
| uuid        | 开通支付服务时申请的appUuid                     |
| orderId     | 第三方业务系统订单号                            |
| fromAddr    | 支付地址                                        |
| toAddr      | 收款地址                                        |
| assetType   | 付款资产类型                                    |
| amount      | 付款金额，字符串类型                            |
| txid        | 支付成功后的链上交易号                          |
| callbackUrl | 付款成功cxc服务通知付款状态的后台回调路径       |
| rsaContent  | 签名数据，使用接入时获得的rsa私钥对参数进行签名 |


### 签名算法

使用APP申请时的rsaPrivatekey运用SHA1WithRSA算法对参数进行签名。

##### 1.按照amount,assetType,callbackUrl,fromAddr,orderId,toAddr,txid,uuid顺序将参数排成json结构。

> amount为字符串类型,直接拼入JSON中,不要转浮点字符串(1.00000)

##### 2.对参数使用SHA1withRSA进行签名，签名后的内容进行BASE64编码即为rsaContent，第三方dapp可用公钥对参数和签名后信息进行验证。

第三方Dapp需要返回的信息如下：

| 返回字段  | 描述                                                 |
| --------- | ---------------------------------------------------- |
| success   | true or false,若为true,回调成功，若为false，回调失败 |
| errorCode | 错误码，若success为true，可不返回.                   |
| errorMsg  | 错误信息，若success为true，可不返回.                 |



## 当前价格查询

接口地址

```
[GET] https://mobile.cxcblock.com:5700/future/oauth/pay/price?currency=USD&securityId=000000
```

频率限制：每分钟限10次

接口参数

| 参数名称   | 参数描述                                    | 是否必填 |
| ---------- | ------------------------------------------- | -------- |
| currency   | 币种类型。目前仅支持CNY,USD,EUR,GBP,JPY,KRW | 是       |
| securityId | 接入方securityId                            | 是       |

返回结果

| 名称      | 描述                                                 |
| --------- | ---------------------------------------------------- |
| success   | true or false,若为true,调用成功，若为false，调用错误 |
| errorCode | 错误码。当success为false时有效                       |
| errorMsg  | 错误信息。当success为false时有效                     |
| result    | 当success为true时得到指定币种当前价格，否则返回空    |

错误码

| 错误码       | 描述             |
| ------------ | ---------------- |
| -pay-price-1 | securityId错误   |
| -pay-price-2 | currency类型错误 |
| -pay-price-3 | 获取失败         |
