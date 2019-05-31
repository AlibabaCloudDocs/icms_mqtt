# 申请 Token 接口 {#concept_54276_zh .concept}

## 接口 {#section_xnc_gm4_hhb .section}

https://\{tokenServerUrl\}/token/apply

## 使用场景 {#section_jwh_hm4_hhb .section}

申请 Token 的接口应该由应用服务器发起，应用服务器验证 MQTT 客户端的权限范围后代替客户端向 MQTT 认证服务器申请 Token。

## 请求参数 {#section_yjk_jm4_hhb .section}

|名称|类型|说明|
|--|--|--|
|actions|String|Token 的权限类型，有“R”（读），“W”（写），“R,W”（读和写） 三种类型。如果同时需要读和写的权限，“R”和“W”之间需要用逗号隔开。|
|resources|String|资源名称，即 MQTT Topic，多个 Topic 以逗号（,）分隔，每个 Token 最多运行操作 100 个资源，当有多个 Topic 时，需要按照字典序排序。 申请 Token 时注册的资源参数支持 MQTT 通配符语法，包含加号单级通配符（+）和井号多级通配符（\#）。

 例如，如果申请 Token 时指定 resources 为 “Topic1/+”，则客户端可以操作 “Topic1/xxx” 的任意 Topic；如果申请 Token 时指定 resources 为 “Topic1/\#”，则客户端可以操作 “Topic1/xxx/xxx/xxx” 的任意多级 Topic。

 |
|accessKey|String|当前请求使用的账号的 AccessKeyId。|
|expireTime|long|Token 失效的毫秒时间戳，允许设置的失效最小间隔是 60 秒|
|proxyType|String|Token 类型，填 MQTT，根据实际产品选择。|
|serviceName|String|填 mq，其他参数无效 。|
|instanceId|String|MQTT 实例 ID，一定要和客户端实际使用的实例 ID 匹配。|
|signature|String|签名字符串，本请求需要计算签名的字段是 actions、resources、expireTime、serviceName、instanceId。|

