# Apply for tokens {#concept_54276_zh .concept}

## Method {#section_xnc_gm4_hhb .section}

https://\{tokenServerUrl\}/token/apply

## Scenario {#section_jwh_hm4_hhb .section}

After verifying the permissions of an MQTT client, the application server calls this method to request a token for this client from the MQTT authorization server.

## Request parameters {#section_yjk_jm4_hhb .section}

|Name|Type|Description|
|----|----|-----------|
|actions|String|The token permission type, which can be R \(read\), W \(write\), or R,W \(read and write\). \*\*Note:\*\* To grant both the read and write permissions, enter R and W and separate them with a comma \(,\).|
|resources|String|The resource name that indicates an MQTT topic. Multiple topics are separated by commas \(,\). Each token can run and operate up to 100 resources. Multiple topics are sorted in the alphabetic order.|
|accessKey|String|The AccessKeyId of the account used in the current request.|
|expireTime|long|The token expiration time, in the timestamp format and precise to milliseconds. The minimum expiration interval is 60 seconds.|
|proxyType|String|The token type, which is set to MQTT here. Set this parameter based on the service that is used.|
|serviceName|String|Set this parameter to mq. Other values are invalid.|
|instanceId|String|The ID of the MQTT instance, which must match the client-used instance ID.|
|signature|String|The signature string. Signature calculation is required by the following fields in the current request: actions, resources, expireTime, serviceName, and instanceId.|

