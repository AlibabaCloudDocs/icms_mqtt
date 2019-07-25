# Token authentication overview {#concept_54226_zh .concept}

Message Queue for MQTT provides MQTT clients with temporary access permissions with a limited validity period through token authentication. The MQTT token service issues temporary credentials to users that are managed by the local account system, and limits the users' access permission. This implements refined permissions control on a per-client and per-resource basis.

As the administrator of the local account, the user applies for a temporary access credential \(Token\) for the actual client, and the client uses the temporary credential for actual business access. Users who manage local accounts can apply for temporary credentials \(tokens\) for clients. Then, the clients use temporary credentials to access services. A token represents the temporary identity of an MQTT client and is assigned permissions, resource lists, and access types by a service administrator. This implements refined permissions control on a per-client and per-resource basis.

|Term|Definition|Description|
|----|----------|-----------|
|Token|Temporary credential|Message Queue for MQTT issues the temporary credential to assign a single MQTT client the permission to access specific resources.|
|AppServer|Application server|The server that enables users to manage local accounts and applies for and manages token services on behalf of clients|
|AuthServer|Authentication Server|Message Queue for MQTT authorization server that processes the token-related requests that are initiated by the application server|

## Procedure { .section}

The token authentication mode is more complex than the signature authentication mode. You must deploy your application server based on the process shown in the following figure, and ensure that the MQTT client is initialized with the capability to interact with the application server to obtain and update tokens.

![](images/45818_en-US.png "Authentication process")

The procedure is as follows:

1.  The MQTT client connects to the application server for authentication upon startup.
2.  The MQTT client applies for the required permissions on all the topics from the application server.
3.  The application server verifies whether the MQTT client is authorized to operate the topics. If the MQTT client is authorized, the application server applies for the resource-related tokens from the authorization server.
4.  Message Queue for MQTT authorization server verifies the token application request and returns the corresponding tokens if the request is valid.
5.  The MQTT client performs local persistence of the tokens that are returned by the application server, and maps required permissions to the tokens. Tokens are cached for the following two purposes:
    -   When the MQTT client is restarted with the same permissions for access, the application server returns the cached tokens to the MQTT client. This avoids repeated token application.
    -   If the authorization server cannot process the client token application request due to an error, the application server returns the previously applied token for local disaster recovery.
6.  The MQTT client sets token parameters based on the specifications to connect to the MQTT broker. The MQTT client can send and receive messages after being authenticated by the MQTT broker.
7.  The MQTT client sends and receives messages properly. If the MQTT broker determines that the token has expired, it triggers an authentication failure and disconnects the MQTT client. In this case, the MQTT client needs to re-apply for a token.

## Client behavior constraints { .section}

-   The MQTT client must set the token as a connection parameter in Password and upload the token when establishing a connection.
-   The MQTT client must know the validity period of the used token and ensure that it is within the validity period. If the token expires, the MQTT broker may disconnect the MQTT client.
-   The MQTT client can listen to the token expiration notifications that are pushed by the MQTT broker, but the MQTT broker does not guarantee that the push is always triggered. Those notifications are only for troubleshooting.
-   The MQTT client must perform persistence of the tokens that are returned by the application server to avoid applying for the same token during each reconnection. Otherwise, the application server may crash when receiving connection requests from many MQTT clients at the same time.
-   The MQTT client can update a token in two ways. One way is to disconnect the MQTT client first and reinitialize the connection using the new token. The other way is to use the MQTT-provided dynamic token update function through system topics. If the token is updated dynamically, the MQTT client must ensure that the local configuration is also updated to avoid that the old token is used during the next connection initialization.

## Application server behavior constraints { .section}

-   The application server must authenticate the MQTT client to prevent the MQTT client from applying for a token by using a forged identity.
-   The application server must manage the token-client relationship to prevent the same MQTT client from calling a token repeatedly.
-   The application server must implement local disaster recovery to prevent service congestion due to temporary failed access to the authorization server.

## Related APIs { .section}

You can call related APIs to implement token authentication.

The application server is responsible for token application and revocation, and interacts with the authorization server by using HTTPS APIs. Each API requires authentication by using an AccessKey and a request signature. Currently, the APIs for token application, revocation, and verification are available.

Message Queue for MQTT clients provide three APIs for dynamic token update, listening to token expiration notifications, and listening to token invalidity notifications, respectively.

## Token service addresses { .section}

Currently, the token service supports public cloud regions in Mainland China and the Singapore region. Finance Cloud is not supported at present. The specific service addresses are as follows:

|Service region|Token server address|
|--------------|--------------------|
|China \(Beijing\)|Mqauth.aliyuncs.com|
|Internet|Mqauth.aliyuncs.com|
|China \(Hangzhou\)|Mqauth.aliyuncs.com|
|China \(Shanghai\)|Mqauth.aliyuncs.com|
|China \(Shenzhen\)|Mqauth.aliyuncs.com|
|Singapore|Mqauth.ap-southeast-1.aliyuncs.com|

