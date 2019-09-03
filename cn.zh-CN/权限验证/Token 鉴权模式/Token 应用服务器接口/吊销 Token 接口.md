# 吊销 Token 接口 {#concept_54277_zh .concept}

## 接口 {#section_wrz_qm4_hhb .section}

https://\{tokenServerUrl\}/token/revoke

## 调用场景 {#section_frc_sm4_hhb .section}

吊销 Token 的接口应该由应用服务器发起，在需要提前中止之前的 Token 授权时调用。

## 使用限制 {#section_fmi_iuv_701 .section}

单用户请求频率限制为 1 次/分钟。

## 请求参数 {#section_npc_tm4_hhb .section}

|名称|类型|说明|
|--|--|--|
|token|String|需要做吊销处理的 Token|
|accessKey|String|当前请求使用的账号的 AccessKeyId|
|signature|String|签名字符串，本请求需要计算签名的字段是 Token|

