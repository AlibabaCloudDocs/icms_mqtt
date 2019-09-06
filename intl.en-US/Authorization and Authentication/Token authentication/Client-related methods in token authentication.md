# Client-related methods in token authentication {#concept_54282_zh .concept}

This topic describes how an MQTT client uploads and updates tokens, and how it listens to notifications about token expiration and invalidity when token authentication is used.

## Set parameters for MQTT client connection in token authentication mode {#section_xyn_vm4_hhb .section}

MQTT supports three token types. Each MQTT client can apply for one token per type at most, and may apply for and use one or more types as needed. The following table lists the token types.

|Type identifier|Description|
|---------------|-----------|
|R|A read-only token that has only the read permission for the specified resources|
|W|A write-only token that has only the write permission for the specified resources|
|RW|A read-write token that has the read and write permissions for the specified resources|

Parameter settings of MQTT client connection in token authentication mode:

**Username:** consists of the authentication mode, AccessKeyId, and InstanceId, which are separated by vertical bars \(|\). The authentication mode is Token in token authentication mode.

**Example**: If MQTT client GID\_Test@@@0001 uses InstanceId mqtt-xxxxx and AccessKeyId YYYYY, the Username parameter for connecting this MQTT client must be set to Token|YYYYY|mqtt-xxxxx in token authentication mode.

**Password:** the content of the token to be used by the MQTT client. The parameter value is a complete string that consists of all the tokens concatenated in sequence by token type and token content that are connected by vertical bars \(|\). Different types of tokens are concatenated in no particular order.

Example 1: If an MQTT client has only the read-only token 123 in the string format, then Password must be set to R|123.

Example 2: If an MQTT client has two token types \(123 as the read-only token and abcd as the write-only token\), then Password must be set to R|123|W|abcd.

**Note:** Ensure that the token parameters of MQTT client connection are set based on the preceding rules and that all the tokens are valid. If some tokens are invalid, the MQTT broker may determine that the token settings are invalid.

## Update tokens for clients {#section_4js_cwy_ny0 .section}

In normal cases, to update the token of an MQTT client, disconnect the MQTT client first and then connect the MQTT client using the new token. If you want to avoid client disconnection during token update, you can replace the token data of the MQTT broker session through dynamic token update.

During dynamic token update, an MQTT client sends a special message by using a predefined system topic to update the new token content to the MQTT broker. The MQTT client must ensure that the local token configuration is updated along with the dynamic token update. Otherwise, the old token data may still be used during the next connection initialization.

Topic sent for token update: $SYS/uploadToken

Content: JSONString

**Content description:**

|Name|Type|Description|
|----|----|-----------|
|Token|String|The MQTT client must upload the token string if token authentication is used.|
|type|String|The token type, which can be W, R, or RW, corresponding to three permission types. An MQTT client can have the three types of tokens at most. An incorrect token type may result in a permission verification error.|

**Response**

A common PubAck message is returned. The MQTT client must wait for the response before performing the publish or subscribe operation. Otherwise, the MQTT broker may still use the old token data for authentication, causing an authentication failure and a disconnection.

## Listen to notifications about imminent token expiration \(subscription not required\) {#section_ruf_u3z_xto .section}

To facilitate service debugging and monitoring, the MQTT broker pushes notifications to the MQTT client by using system topics to notify the client of tokens that will soon expire. The MQTT client can listen to such notifications to detect whether tokens are about to expire.

Received topic: $SYS/tokenExpireNotice

Content: JSONString

**Content description:**

|Name|Type|Description|
|----|----|-----------|
|expireTime|Long|The token expiration time, in the timestamp format and precise to milliseconds. The MQTT client is typically notified of the token expiration time once five minutes in advance. However, the MQTT broker does not guarantee that a notification is always sent.|
|type|String|The client-uploaded token type, which can be W, R, or RW, corresponding to three permission types.|

**Response:**

After receiving a notification about imminent token expiration, the MQTT client needs to apply for a new token as soon as possible to avoid failed transmission of messages.

## Listen to notifications about invalid tokens \(subscription not required\) {#section_ekg_cd4_lug .section}

To facilitate service debugging and monitoring, the MQTT broker pushes notifications to the MQTT client by using system topics to notify the client of token authentication errors. The MQTT client can listen to such notifications to detect token mismatch and other permission errors.

Received topic: $SYS/tokenInvalidNotice

Content: JSONString

**Content description:**

|Name|Type|Description|
|----|----|-----------|
|code|Integer|The type of a token verification error.|
|type|String|The client-uploaded token type, which can be W, R, or RW, corresponding to three permission types.|

**Response:**

Authentication may fail when the MQTT broker encounters a token verification error, and then the MQTT broker proactively disconnects the MQTT client. The MQTT broker pushes an error code to the MQTT client before disconnecting the MQTT client. The MQTT client can identify the cause based on the error code.

|Type code|Error type|
|---------|----------|
|1|The token is forged and cannot be parsed.|
|2|The token has expired.|
|3|The token has been revoked.|
|4|The resource and token do not match.|
|5|The permission type and token do not match.|
|8|The signature is invalid.|
|-1|The account permission is invalid.|

