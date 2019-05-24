# 校验 Token 接口 {#concept_54278_zh .concept}

## 接口 {#section_n5y_mm4_hhb .section}

https://\{tokenServerUrl\}/token/query

## 使用场景 {#section_mpb_4m4_hhb .section}

校验 Token 的接口应该由应用服务器发起，应用服务器可以使用本接口确认单个 Token 是否有效。

## 请求参数 {#section_rvn_pm4_hhb .section}

|名称|类型|说明|
|--|--|--|
|token|String|需要查询的 Token|
|accessKey|String|当前请求使用的账号的 AccessKeyId|
|signature|String|签名字符串，本请求需要计算签名的字段是 Token|

