# Authentication overview {#concept_54225_zh .concept}

This topic describes the principle behind and the types of authentication of MQTT clients.

## Authentication principle {#section_ems_sf4_hhb .section}

When an MQTT client sends and receives messages, the MQTT broker authenticates the MQTT client based on the Username and Password parameters. The definitions of the Username and Password parameters vary depending on different scenarios of permission verification. A proper authentication mode must be selected during MQTT client development based on the actual situation, and the Username and Password parameters must be set correctly based on the corresponding specifications.

**Authentication modes**

|Mode|Description|Scenarios|
|----|-----------|---------|
|Signature|Signature verification|Permanent authorization, applicable to secure and trusted clients|
|Token|On-demand token permission verification|On-demand authorization, applicable to untrusted clients|

**Username**

The Username parameter consists of the authentication mode, AccessKeyId, and InstanceId, which are separated by vertical bars \(|\).

Example: If MQTT client GID\_Test@@@0001 uses InstanceId mqtt-xxxxx and AccessKey YYYYY, the Username parameter for connecting this MQTT client must be set to Signature|YYYYY|mqtt-xxxxx in signature authentication mode and to Token|YYYYY|mqtt-xxxxx in token authentication mode.

**Password**

|Permission verification mode|Password|Scenarios|
|----------------------------|--------|---------|
|Signature verification|Client ID signature encoded in Base64. For the setting method, see the document about signature calculation.|Permanent authorization, applicable to secure and trusted clients|
|On-demand token permission verification|Uploaded token content. For the setting method, see the document about token calculation.|On-demand authorization, applicable to untrusted clients|

## Types and comparison of authentication modes {#section_xab_h0d_ns2 .section}

Currently, MQ for MQTT supports signature authentication and token authentication, which correspond to fixed permissions and non-fixed on-demand permissions, respectively. The two modes and scenarios are detailed as follows.

**Signature authentication \(fixed permissions\)**

The signature authentication mode is the default authentication mode recommended by MQ for MQTT. In this mode, MQTT clients of the same type are assigned the same permissions and calculate signatures by using the same account. The MQTT broker identifies the accounts and permissions of the MQTT clients through signature comparison. The signature authentication mode is easy to understand and use.

-   Scenarios

    The used MQTT clients can be logically classified into the same type, belong to the same account, and have the same permissions. The runtime environment of each MQTT client is relatively controllable. You do not have to worry about malicious attacks against devices, such as cracking and stealing.

    From the service perspective, MQTT clients of the same type are managed by the same account, which can be a primary account or a sub-account. Therefore, the MQTT clients only need to calculate signatures based on the corresponding AccessKeySecret. Permissions are managed in the console by the account administrator.

-   Signature calculation

    Signatures are calculated by using each MQTT client's independent information to prevent signature stealing. MQ for MQTT requires each MQTT client to use its unique client ID as the to-be-signed content. For the signature calculation method, see .


**Token authentication \(on-demand permissions\)**

The token authentication mode supports access through on-demand credentials and is applicable in the scenario where the permissions of each MQTT client need to be classified with a small granularity or MQTT clients need to be assigned only on-demand permissions with a limited validity period. The token service allows you to set the resources that are accessed by a single MQTT client, the permission level that is assigned to the MQTT client, and the permission expiration time.

For more information about the process and precautions of token authentication, see [Signature authentication](intl.en-US/Authorization and Authentication/Signature authentication.md#).

-   **Scenarios**

    The service party has an independent local account system and needs to split the identities of Alibaba Cloud accounts for the second time, or even needs to assign an independent account and permissions to each MQTT client. In this case, the RAM user system of Alibaba Cloud cannot meet the requirement for refined classification of MQTT clients.

    Though the MQTT clients belong to the same Alibaba Cloud account, they need to play different roles assigned by local accounts \(owned by the service department or a single MQTT client\). This requirement cannot be met by using Alibaba Cloud accounts in signature authentication mode. The use of fixed permissions cannot meet the requirements of mobile terminals that need to protect against cracking and hijacking. Use of only on-demand and non-fixed permissions makes it more flexible to control permissions on a per-client basis and assign permissions on a per-resource basis.

-   **Token usage**

    The token authentication mode is relatively complex. The service party needs to have the account \(device\) management capability, manage the permissions and permission validity period of each device, apply for tokens on secure and controllable application servers and authorization servers, and deliver the tokens to MQTT clients. For usage instructions, see the document about the token service.


