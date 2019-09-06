# 应用服务器 Token 接口规范 {#concept_54274_zh .concept}

您的应用服务器访问认证服务器的 Open API 接口时，需要遵循一定的参数和返回值获取规范，否则会无法调用接口，具体规范如下：

## 签名计算 {#section_oyx_yl4_hhb .section}

应用服务器操作申请 Token 等接口，服务端会对调用参数进行参数校验并过滤非法请求。应用服务器需要按照如下规则将请求参数按照指定方式排序拼接，组织成待签名字符串，然后使用秘钥 AccessKeySecret 加密得到签名。

**说明：** 此处的签名计算规则和 MQTT 客户端的签名校验是不同的逻辑，遵循不同的签名计算规则。

具体约束规则如下：

-   参数对使用 key=value 格式；
-   参数对之间使用“&”分隔；
-   计算签名时，需要将参数对按照 key 的字典序排序。
-   参数对内部，如果同一个 key 有多个 value，即同名参数，也需要使用字典序排序，按序使用“,”连接。

## 示例 { .section}

比如原始请求的 HTTP 方法是：

```
https://mqauth.aliyuncs.com/token/apply
			
```

参数列表有：

```
parama=a
paramc=c2,c1
paramb=b2,b1,b3
			
```

那么先将参数按照 key 排序，得到：

```
parama=a
paramb=b2,b1,b3
paramc=c2,c1
			
```

再将同一参数内的值也按照字典序排序，此处“b2,b1,b3”是乱序的，因此排序参数得到：

```
parama=a
paramb=b1,b2,b3
paramc=c1,c2
```

最终拼接得到待签名字符串：

```
parama=a&paramb=b1,b2,b3&paramc=c1,c2
```

然后使用 secretKey 作为密钥，HmacSHA1 算法计算得到签名。

**注意：**

-   HTTP 管理 Token 时，可以选用 HTTP 或者 HTTPS，应用自行选择。
-   HTTP 方法可以选择 GET 或者 POST，建议生产环境使用 POST，测试环境可以使用 GET。

## 返回状态码 { .section}

Token Open API 请求到达认证服务器后，正常处理后会返回给调用方响应，类型是 JSON 字符串，调用方可根据需求取出 JSON 中特定的值。

JSON 字符串的 Key-Value 映射如下：

|Key|类型|说明|
|---|--|--|
|success|boolean|是否成功。|
|message|String|处理结果信息，或者异常描述。|
|code|int|返回码，可根据 code 判断当前请求处理情况或者错误类型。|
|tokenData|String|Token 字符串，申请 Token 的接口会返回该值。|

其中，错误码列表如下：

|Code|说明|
|----|--|
|200|成功|
|400|参数校验错误|
|407|签名计算错误|
|409|Token 生成处理错误|
|410|Token 吊销处理失败|
|1|伪造 Token，不可解析|
|2|Token 已经过期|
|3|Token 已经被吊销|

