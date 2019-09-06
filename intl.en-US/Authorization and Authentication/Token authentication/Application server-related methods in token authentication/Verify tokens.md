# Verify tokens {#concept_54278_zh .concept}

## Method {#section_n5y_mm4_hhb .section}

https://\{tokenServerUrl\}/token/query

## Scenario {#section_mpb_4m4_hhb .section}

This method is called by the application server to check whether a single token is valid.

## Request parameters {#section_rvn_pm4_hhb .section}

|Name|Type|Description|
|----|----|-----------|
|token|String|The token that you want to verify.|
|accessKey|String|The AccessKeyId of the account used in the current request.|
|signature|String|The signature string. The Token field in the current request requires signature calculation.|

