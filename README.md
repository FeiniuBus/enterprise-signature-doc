## 欢迎使用飞牛巴士企业签名

本文将会为您描述签名操作的技术要领。

除此之外，我们还提供开箱即用的签名工具库。（[下载资源](https://github.com/FeiniuBus/enterprise-signature-doc/releases)）

工具库目前支持以下语言：
  * Java 1.5+ （[范例](https://github.com/FeiniuBus/signature-sample-java)）

本文只提供签名相关的信息，API信息请另行查阅。

### 准备工作

在开始之前，请确认您已经从我公司获得供第三方使用的用户凭证（APPID和APPSECRET）。

### 签名要素（Payload）
签名要素是组成签名内容的各个部分，目前签名要素由5个部分组成：
  * `Timestamp` : 发起请求时的Unix时间戳，精确到 `秒`
  * `APPID` : 我公司提供的32位字符串ID
  * `RequestMethod` : 发起请求的方法
  * `RequestUrl` : 发起请求的地址
  * `RequestBody` : 发起请求的请求体，可以为空
 
签名要素需要按以下格式组成一个字节数组（Byte Array）:
![image](https://github.com/FeiniuBus/enterprise-signature-doc/raw/master/%E4%BC%81%E4%B8%9A%E7%BD%91%E5%85%B3%E7%AD%BE%E5%90%8D.jpg)
   
### 签名算法
得到签名要素(Payload)后，使用我公司提供的 `APPSECRET` 作为密钥，使用 `HMACSHA256` 算法计算 `Payload` 的摘要，至此签名完成。
