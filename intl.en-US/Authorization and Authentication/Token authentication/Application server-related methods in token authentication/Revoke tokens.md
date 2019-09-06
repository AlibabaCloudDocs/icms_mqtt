# Revoke tokens {#concept_54277_zh .concept}

## Method {#section_wrz_qm4_hhb .section}

https://\{tokenServerUrl\}/token/revoke

## Scenario {#section_frc_sm4_hhb .section}

This method is called by the application server to terminate previous token authorization in advance.

## Request parameters {#section_npc_tm4_hhb .section}

|Name|Type|Description|
|----|----|-----------|
|token|String|The token to be revoked.|
|accessKey|String|The AccessKeyId of the account used in the current request.|
|signature|String|The signature string. The Token field in the current request requires signature calculation.|

