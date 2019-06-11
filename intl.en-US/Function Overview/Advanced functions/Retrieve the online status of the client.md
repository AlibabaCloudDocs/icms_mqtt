# Retrieve the online status of the client {#concept_50069_zh .concept}

You can retrieve the current online status of the AliwareMQ for IoT client \(referred to here as the client\) by using the synchronous query method or asynchronous online/offline notification method.

AliwareMQ for IoT must be used with backend MQ services \(such as AliwareMQ for RocketMQ\) and work with applications that are deployed on cloud servers \(hereinafter referred to as service applications\) to complete service processes.

The main scenarios for retrieving the online status of the client are as follows:

-   The subsequent operation logic needs to be determined based on whether the client is online or not during the main service process.
-   The online status of a specific client needs to be determined during the O&M process.
-   Service applications need to trigger some predefined actions when a client goes online or offline.

## Basic principle {#section_1e4_ef7_7m2 .section}

The AliwareMQ for IoT broker supports the following methods for retrieving the online status of the client:

-   Call the synchronous query interface

    This method is relatively simple. You can call an HTTP/HTTPS API through an open endpoint to query the online status of a specific client. This method can only query the status of a single client.

-   Asynchronous online/offline notification

    This method uses notification messages. When a client goes online or offline, the MQTT broker pushes an online or offline message to backend MQ. Service applications are generally deployed on Alibaba Cloud ECS instances. Service applications can subscribe to this message from backend MQ to retrieve the online/offline events of all clients.

    This method perceives the client status asynchronously and detects the online and offline events rather than the online status of the client. Therefore, cloud-based applications need to analyze the client status based on the timeline of a series of events.


The differences between both methods are as follows:

-   The synchronous query method queries the real-time status of a client. Theoretically, it is more precise than the asynchronous notification method, but it only supports the status query of a single client at one time.
-   Asynchronous online/offline notification messages are based on message decoupling, so status determination is more complex and more likely to be incorrect. However, this method can analyze the running status traces of multiple clients based on events.

## Synchronous query method \(under public beta\) {#section_rt0_d03_tdn .section}

The synchronous query method is currently under public beta and is only available in the Internet beta test region. It will be available in other regions in the future. The MQTT broker exposes the query method through HTTP/HTTPS.

-   **Query method**

    ``` {#codeblock_xg4_k77_yd7}
    https://<mqtt-xxx.mqtt.aliyuncs.com\>/route/clientId/get 
    ```

    Specifically:

    -   <mqtt-xxx.mqtt.aliyuncs.com\> must be replaced with the endpoint domain of the instance that you purchased.
    -   Both HTTP and HTTPS are supported for protocols. Only the GET method is supported for calls.
-   **Request parameters**

    [Table 1](#table_zpr_wtx_i4w) lists the parameters included in a sent request.

    |Name|Type|Description|
    |----|----|-----------|
    |accessKey|String|The AccessKeyId of the account that is used in the current request.|
    |resource|String|The ID of the client to be queried. Only the client IDs under the current account can be queried.|
    |timestamp|Long|The parameter used to block invalid and expired requests. It must be set to the current actual time, that is, the UNIX timestamp of the current request, in milliseconds. The expiration time of the timestamp is 5 minutes before or after the current actual time.|
    |instanceId|String|The ID of the current instance.|
    |signature|String|The signature of the request parameters, which is calculated based on the AccessKeySecret of the account, with other parameters used as strings to be signed. It is used for permission verification and the prevention of request tampering. For more information about the calculation method, see [Specifications on token-related methods for the application server](../intl.en-US/Authorization and Authentication/Token authentication/Application server-related methods in token authentication/Specifications on token-related methods for the application server.md#).|

-   **Responses**

    After receiving a query request, the MQTT broker verifies the parameters and returns the number of online connections of the current client for a valid query request. [Table 2](#table_d6x_mdp_6aa) lists the possible responses.

    |HTTP status code|Description|
    |----------------|-----------|
    |400|This code is returned if the request is invalid. The possible cause is that the URL is incorrect or a parameter is missing.|
    |403|This code is returned if permission verification fails. The possible cause is that the signature calculation is incorrect or that the current account does not have the permission to query the status of the client in the request.|
    |200|This code is returned if the request is successfully processed.|

    The response includes the HTTP status code and the HTTP content, which contains the corresponding results in JSON format. [Table 3](#table_780_eho_lqr) lists the response parameters.

    |Name|Type|Description|
    |----|----|-----------|
    |code|Integer|The status code of the request processing result, which indicates whether the current request is successful and the cause in the case of failure.|
    |success|Boolean|Indicates whether the query is successful. Valid values:     -   true: The query is successful.
    -   false: The query fails.
 |
    |online|Integer|The online status of the client and the number of maintained TCP connections. Valid values:     -   0: The client is offline.
    -   A value greater than 0: The client is online. Note that a value greater than 1 may indicate repeated connections. Due to data synchronization latency on the MQTT broker, there is a small possibility that the value -1 may be returned, indicating that the query result is unknown. In that case, we recommend that you retry the request or make a conservative judgment based on the actual scenario.
 |
    |message|String|The description of the request processing result, which is used to identify the cause of an exception.|

    [Table 4](#table_8s9_5pu_c8j) describes the error codes.

    |Code|Description|
    |----|-----------|
    |0|This code is returned if the request is successfully processed.|
    |1|This code is returned if a parameter is missing. In this case, we recommend that you check whether parameter pairs are complete.|
    |2|This code is returned if an error occurs during signature calculation. In this case, we recommend that you check the signature calculation process.|
    |3|This code is returned if the permission verification fails. In this case, we recommend that you check whether the current account has created the group ID of the device.|
    |4|This code is returned if the request timestamp expires. In this case, we recommend that you check whether the value of timestamp is within the last 5 minutes.|


## Asynchronous online/offline notification {#section_grc_4n0_lcy .section}

As described in [Basic principle](#section_1e4_ef7_7m2), if the asynchronous online/offline notification method is used, online and offline events are mapped to backend MQ.

The following uses AliwareMQ for RocketMQ as backend MQ to demonstrate this.

**Procedure**

1.  Create a topic that corresponds to online and offline events.

    In the AliwareMQ for IoT console, create a topic for the device with the target group ID. For information about how to create a topic, see **Create a topic and group ID** in [Quick start guide](../intl.en-US/Quick Start/Quick start guide.md#).

    For example, if the target client type corresponds to the group ID GID\_XXX, then the client ID and topic for this client type are GID\_XXX@@@YYYYY and GID\_XXX\_MQTT, respectively.

    Specifically:

    -   GID\_XXX indicates the group ID created in the AliwareMQ for IoT console.
    -   YYYYY indicates the device ID and is concatenated with the group ID to form the client ID in the format of <GroupID\>@@@<DeviceID\>.
    -   \_MQTT indicates the fixed suffix that is required in the name of the topic for this type of event notifications.
    For more information, see [Terms](../intl.en-US/Product Introduction/Terms.md#).

2.  Service applications subscribe to this type of notifications.

    Use the topic created in [step 1](#li_p5g_4qs_qnj) to receive the online and offline events of the target client. For information about how to receive messages from AliwareMQ for RocketMQ, see [Subscribe to messages](https://help.aliyun.com/document_detail/29551.html?spm=a2c4g.11186623.2.17.b6d83bcdcXvtmD).

    The event type is included in a RocketMQ message tag, indicating whether it is an online or offline event. The data format is as follows:

    `RocketMQ Tag: connect/disconnect/tcpclean`

    Specifically:

    -   A connect event indicates an online event of the client.
    -   A disconnect event indicates that the client proactively disconnects from the MQTT broker. According to MQTT, the client sends a disconnect packet before proactively releasing the TCP connection. The MQTT broker triggers the disconnect message after receiving the disconnect packet. If a client SDK does not send the disconnect packet, the MQTT broker cannot receive the disconnect message.
    -   A tcpclean event indicates that the current TCP connection is disconnected. If the current TCP connection is disconnected, a tcpclean event is triggered regardless of whether the client has explicitly sent a disconnect packet.
    **Note:** 

    A tcpclean message indicates that the network-layer connection of the client is disconnected. A disconnect message only indicates that the client proactively sends a disconnect packet. Some clients may fail to send a disconnect message due to unexpected exit. This is subject to the implementation on the clients. Therefore, determine whether a client is offline based on the tcpclean event.

    Data content is of the JSON type and related keys are as follows:

    -   clientId indicates a specific device.
    -   time indicates the event time.
    -   eventType indicates the event type, which is used by clients to differentiate events.
    -   channelId uniquely identifies each TCP connection.
    -   clientIp indicates the Internet egress IP address used by a client.
    Example:

    ``` {#codeblock_dyb_ddu_lpf}
    clientId: GID_XXX@@@YYYYY
    time:1212121212
    eventType:connect/disconnect/tcpclean
    channelId:2b9b1281046046faafe5e0b458e4XXXX
    clientIp: 192.168.X.X:133XX     
    ```

    To determine whether a client is online, check the last received message while considering the context of online/offline notification messages.

    The determination rules are as follows:

    -   The sequence of online and offline events generated by clients with the same client ID is determined by time. That is, a more recent event has a greater timestamp.
    -   Clients with the same clientId may be transiently disconnected multiple times. Therefore, when an offline notification message is received, you need to check whether the message is related to the current TCP connection based on the channelId field. In short, an offline notification message can only overwrite an offline notification message with the same channelId. If channelIds of offline notification messages are different, the offline notification messages cannot be overwritten even if the value of time is more recent.

