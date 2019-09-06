# Token 客户端接口 {#concept_54282_zh .concept}

本文介绍微消息队列 MQTT 客户端在 Token 模式下如何上传 Token、更新 Token 、监听 Token 失效信息、监听 Token 非法信息。

## Token 模式 MQTT 客户端连接参数设置 {#section_xyn_vm4_hhb .section}

MQTT 支持三种模式的 Token，每个客户端每个类型至多申请一个 Token，根据实际需要可能申请其中的一种或者多种，并使用。具体的类型如下表所示。

|类型标志|说明|
|----|--|
|R|只读类型的 Token，只拥有指定资源的读权限|
|W|只写类型的 Token，只拥有指定资源的写权限|
|RW|读写类型的 Token，对指定资源既可以读也可以写|

Token 模式下 MQTT 客户端的连接参数设置如下：

**Username:** 由鉴权模式名称、AccessKeyId、InstanceId 三部分组成，以 “|” 分隔。Token 模式下鉴权模式设置为”Token”。

**举例**：一个客户端的 ClientId 是 GID\_Test@@@0001，使用了实例 ID 是 mqtt-xxxxx，使用了 AccessKeyId 是 YYYYY，则 Token 模式的 UserName 应该设置成 “Token|YYYYY|mqtt-xxxxx”。

**Password:** 客户端需要使用的 Token 内容。具体设置方法是将所有的 Token 按照 Token 类型和 Token 内容依次使用“|”连接符拼接成一个完整的字符串，不同类型之间的 Token 拼接顺序无要求。

举例 1：客户端只有一个读类型的 Token，Token 字符串为“123”，则 Password 为“R|123”。

举例 2：客户端拥有 2 个类型的 Token，读类型的 Token 是“123”，写类型的 Token 是“abcd”，则 Password 为“R|123|W|abcd”。

**说明：** 客户端设置 Token 参数时需要保证严格按照约定规则，且需要保证所有 Token 有效，如果仅用部分 Token 合法，服务端仍然会判定非法。

## 客户端更新 Token 凭证 {#section_rc4_6uw_n5c .section}

正常情况下客户端更换 Token 时需要断开连接重新使用新的 Token 来连接，如果业务场景中不希望由于更换 Token 中断客户端的连接，可以使用动态更新 Token 的接口来动态替换服务端 session 内的 Token 数据。

动态更新 Token 本质上是由 MQTT 客户端以约定的系统 Topic 发送一个特殊的消息，将新的 Token 内容更新到服务端，客户端需要保证在动态替换的同时，本地配置也要替换，防止下次连接初始化又使用了旧的 Token 数据。

更新 Token 发送 Topic：$SYS/uploadToken

内容：JSONString

**内容信息：**

|名称|类型|说明|
|--|--|--|
|token|String|如果客户端选用 Token 模式，则需要上传 Token 字符串。|
|type|String|Token 类型，分为 W、R、RW 共三种，对应三种权限类型的 Token。一个客户端最多拥有这 3 个 Token，设置错误的类型会导致权限校验错误。|

**返回值**

普通的 PubAck 报文。客户端必须等到该响应才能进行下一步 Pub 或者 Sub 操作，否则服务端仍然有可能使用旧的 Token 数据来做鉴权，可能会鉴权失败导致连接断开。

## 客户端监听即将失效的 Token 信息（无需订阅） {#section_s35_jon_mia .section}

服务端为方便业务调试和监控，会在 Token 即将失效的时候以系统 Topic 的形式推送通知消息到 MQTT 客户端，客户端可以监控该消息来判断是否有出现过 Token 即将到期的情况。

接收 Topic：$SYS/tokenExpireNotice

内容：JSONString

**内容信息：**

|名称|类型|说明|
|--|--|--|
|expireTime|Long|该 Token 即将于什么时候失效，格式为毫秒时间戳，一般提前 5 分钟通知，只通知一次，但服务端不保证一定会有通知。|
|type|String|Token 类型，分为 W、R、RW 共三种，对应客户端上传的三种权限类型的 Token。|

**响应：**

客户端收到 Token 即将失效的消息后，需要尽快处理重新申请 Token 的动作，以免造成收发消息失败。

## 客户端监听 Token 非法的通知（无需订阅） {#section_03j_h0a_zz1 .section}

服务端为方便业务调试和监控，会在 Token 鉴权错误时以系统 Topic 的形式推送通知消息到 MQTT 客户端，客户端可以监控该消息来判断是否有出现过 Token 不匹配等错误权限的情况。

接收 Topic：$SYS/tokenInvalidNotice

内容：JSONString

**内容信息：**

|名称|类型|说明|
|--|--|--|
|code|int|Token 校验失败的类型。|
|type|String|Token 类型，分为 W、R、RW 共三种，对应客户端上传的三种权限类型的 Token。|

**响应：**

服务端校验 Token 失效，会导致鉴权失败，服务端会主动断开链接。断开链接之前，服务端会给客户端推送失败的 Code，客户端根据 Code 即可判断原因。

|type code|错误类型|
|---------|----|
|1|伪造 Token，不可解析|
|2|Token 已经过期|
|3|Token 已经被吊销|
|4|资源和 Token 不匹配|
|5|权限类型和 Token 不匹配|
|8|签名不合法|
|-1|帐号权限不合法|

