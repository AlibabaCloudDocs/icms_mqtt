# Signature authentication {#concept_g5d_mk4_hhb .concept}

This topic describes how to calculate a signature and create a signature in the console when signature authentication is used by MQ for MQTT.

## Use the MQTT SDK to access the MQTT broker {#section_k3l_4k4_hhb .section}

If signature verification is used, the Connect message that the MQTT client sends for connecting to the MQTT broker must set the Username and Password parameters based on the specifications described here. The setting and calculation methods are as follows:

**Username**

The Username parameter consists of the authentication mode, AccessKeyId, and InstanceId, which are separated by vertical bar \(|\). The authentication mode is set to Signature in signature authentication mode.

Example: If MQTT client GID\_Test@@@0001 uses InstanceId mqtt-xxxxx and AccessKeyId YYYYY, the Username parameter for connecting this MQTT client must be set to Signature|YYYYY|mqtt-xxxxx in signature authentication mode.

**Password**

The Password parameter indicates the result of client ID signing. The calculation method is as follows:

For example, a client ID is GID\_AAA@@@BBB001.

Calculate the signature of the to-be-signed string by using AccesKeySecret and HMAC SHA-1 to obtain a binary array. Encode the binary array in Base64 to obtain the final signature string Password, that is, eqweq+adwe23fssf.

Each language provides a function library to implement the HMAC algorithm. You can also refer to the demo project.

## Verify the signature in the console {#section_ijc_5k4_hhb .section}

The MQ for MQTT console provides the signature calculation tool so that you can compare and check whether your signature calculation is correct.

In the left-side navigation pane of the MQ for MQTT console, choose **Signature Verification**, and enter the AccessKeyId, AccessKeySecret, and Client ID of the application-used account to obtain the Username and Password parameters that need to be set in the application.

**Note:** 

The tool uses only frontend JavaScript of the web browser for calculation and does not transmit AccessKeySecret to RocketMQ, removing the risk of AccessKeySecret disclosure. In the actual situation, the tool is only used by the console for troubleshooting and data comparison.

Calculate the signature on the MQTT client. Alternatively, calculate on the MQTT broker and then send the result to the MQTT client for security purposes.

