## 欢迎使用飞牛巴士企业签名

本文将会为您描述签名操作的技术要领。

除此之外，我们还提供开箱即用的签名工具库。

工具库目前支持以下语言：
  * Java 1.5+ 
  * PHP 5+
  * .NetCore 2.0+
  * .NetFramework 3.5+

本文只提供签名相关的信息，API信息请另行查阅。

### 准备工作

在开始之前，请确认您已经从我公司获得供第三方使用的用户凭证（APPID和APPSECRET）。

### 签名要素（Payload）
签名要素是组成签名内容的各个部分，目前签名要素由5个部分组成：
  * `Timestamp` : 发起请求时的Unix时间戳，精确到 `秒`
  * `APPID` : 我公司提供的32位字符串ID
  * `RequestMethod` : 发起请求的方法
  * `RequestParam` : URL参数
  * `RequestUrl` : 发起请求的地址
  * `RequestBody` : 发起请求的请求体，可以为空
 
签名要素需要按以下格式组成一个字节数组（Byte Array）:
![image](https://github.com/FeiniuBus/enterprise-signature-doc/raw/master/%E4%BC%81%E4%B8%9A%E7%BD%91%E5%85%B3%E7%AD%BE%E5%90%8D.jpg)
   
### 使用范例
** 请先行从我公司获取相关SDK **

#### Java
##### Get 请求

```java
  String APP_ID ="我司提供的APPID";
  String APP_SECRET = "我司提供的APPSECRET";

  QueryString qs = new QueryString();
  qs.add("adcode", "510100"); //Url参数

  Payload payload = new Payload();
  payload.setAppId(APP_ID);
  payload.setRequestMethod(RequestMethod.GET);
  payload.setRequestUrl("请求的服务URL，例如 /bus/type");
  payload.setQueryString(qs);
  payload.setTimestamp(TIMESTAMP); //当前时间的UNIX时间戳

  String signature = SignatureUtil.sign(payload, APP_SECRET);

  System.out.println(signature);
```

##### POST 请求

```java
  String APP_ID ="我司提供的APPID";
  String APP_SECRET = "我司提供的APPSECRET";

  Payload payload = new Payload();
  payload.setAppId(APP_ID);
  payload.setRequestMethod(RequestMethod.POST);
  payload.setRequestUrl("请求的服务URL，例如 /bus/type");
  payload.setContent("请求体JSON字符串".getBytes("utf-8")); //务必使用UTF-8编码集
  payload.setTimestamp(TIMESTAMP); //当前时间的UNIX时间戳

  String signature = SignatureUtil.sign(payload, APP_SECRET);

  System.out.println(signature);
```

#### .NetCore & .NetFramwork (C#)
##### GET 请求

```csharp
  private const string AppId="我司提供的APPID";
  private const string AppSecret="我司提供的APPSECRET";

  var qs = new QueryString();
  qs.Add("adcode", "510100"); //Url参数

  var payload = new Payload
  {
    AppId = Configuration.Instance.AppId
  };
  payload.RequestMethod = RequestMethod.Get;
  payload.RequestUrl = "请求的服务URL，例如 /bus/type";
  payload.QueryString = qs.ToString();
  payload.Timestamp=TIMESTAMP; //当前时间的UNIX时间戳
  
  var signature = SignatureUtil.Sign(payload, AppSecret);

  Console.WriteLine(signature);
```

##### POST 请求

```csharp
  private const string AppId="我司提供的APPID";
  private const string AppSecret="我司提供的APPSECRET";

  var payload = new Payload
  {
    AppId = Configuration.Instance.AppId
  };
  payload.RequestMethod = RequestMethod.Get;
  payload.RequestUrl = "请求的服务URL，例如 /bus/type";
  payload.Content = Encoding.UTF8.GetBytes("请求体JSON字符串"); //务必使用UTF-8编码集
  payload.Timestamp=TIMESTAMP; //当前时间的UNIX时间戳
  
  var signature = SignatureUtil.Sign(payload, AppSecret);

  Console.WriteLine(signature);
```

#### PHP
##### GET 请求

```php
<?php
  header("content-type:text/html;charset=utf8");  //务必使用UTF-8编码集

  //此处应当include所有SDK代码文件

  $appId = "我司提供的APPID";
  $appSecret = "我司提供的APPSECRET";

  $queryString = new FeiniuBusQueryString();
  $queryString->append("adcode", "510100"); //Url参数

  $payload = new FeiniuBusPayload();
  $payload->AppId=$appId;
  $payload->RequestUrl="/bus/type";
  $payload->RequestMethod="GET";
  $payload->QueryString=$queryString->toString();
  $payload->Content="";
  $payload->Timestamp="TIMESTAMP"; //当前时间的UNIX时间戳

  $util = new FeiniuBusSignatureUtil();
  echo $util->sign($payload, $appSecret);
```

##### POST 请求

```php
<?php
  header("content-type:text/html;charset=utf8");  //务必使用UTF-8编码集

  //此处应当include所有SDK代码文件

  $appId = "我司提供的APPID";
  $appSecret = "我司提供的APPSECRET";

  $payload = new FeiniuBusPayload();
  $payload->AppId=$appId;
  $payload->RequestUrl="/bus/type";
  $payload->RequestMethod="POST";
  $payload->QueryString="";
  $payload->Content="请求体JSON字符串";
  $payload->Timestamp="TIMESTAMP"; //当前时间的UNIX时间戳

  $util = new FeiniuBusSignatureUtil();
  echo $util->sign($payload, $appSecret);
```