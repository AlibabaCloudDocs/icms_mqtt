# Terms {#concept_42420_zh .concept}

Before using Message Queue for MQTT, you must understand the concepts and terms related to this service and the MQTT protocol. For information about the use cases, see [What is Message Queue for MQTT?](intl.en-US/Product Introduction/What is Message Queue for MQTT?.md#).

## Concepts {#section_0wq_m9y_osd .section}

 Instance
 :   An entity that you create when you purchase Message Queue for MQTT. Each Message Queue for MQTT instance maps to a globally unique endpoint URL. Before using Message Queue for MQTT, you must create an instance in the corresponding region and access the service through the corresponding endpoint. For information about how to create an Message Queue for MQTT instance, see [Quick start guide](../intl.en-US/Quick Start/Quick start guide.md#).

  MQTT broker
 :   An Message Queue for MQTT broker that provides interaction based on the MQTT protocol and exchanges messages with an MQTT client and MQ for MQ.

  MQTT client
 :   A mobile node that interacts with the MQTT broker. It is short for Message Queue for MQTT client.

  P2P message
 :   A special type of message that is provided by Message Queue for MQTT based on the standard MQTT protocol. This type of message can be directly sent to a specified target MQTT client without subscription matching. For more information, see [P2P messaging model](../intl.en-US/Function Overview/Advanced functions/P2P messaging model.md#).

  Parent topic
 :   MQTT is a messaging protocol that is based on the publish-subscribe model. Therefore, each message belongs to a topic. MQTT supports multiple levels of topics. A level-1 topic is a parent topic. Before using Message Queue for MQTT, you must create a parent topic in the Message Queue for MQTT console or MQ console.

  Subtopic
 :   A level-2 or level-3 topic is a subtopic of a parent topic in MQTT. You can directly set subtopics in the code without having to create them in the console. Note that the total length of a parent topic and its subtopics cannot exceed 64 characters in Message Queue for MQTT. If it is too long, a client exception may occur.

  Client ID
 :   The globally unique identifier of each client. If two clients with the same ID are used to connect to the Message Queue for MQTT service, the request will be denied.

    A client ID consists of two parts in the format of <GroupID\>@@@<DeviceID\>. A client ID can contain up to 64 characters and must not contain invisible characters. For more information, see [EN-US\_TP\_152380.md\#](intl.en-US/SDK Reference/Limits.md#).

  Group ID
 :   An identifier that specifies the name of a group of nodes with identical logic and functions, representing a category of devices with the same functions. To use a group ID, you must first create it in the MQ for MQTT console. For information about how to create a group ID, see [Quick start guide](../intl.en-US/Quick Start/Quick start guide.md#).

  Device ID
 :   A unique identifier for each device that you specify. A device ID must be globally unique, for example, the serial number of a sensor.

 ## Network type {#section_0t3_wdl_hk0 .section}

 Endpoint URL
 :   For Message Queue for MQTT, both Internet and intranet endpoints are supported. We recommend that mobile terminals use Internet endpoints. Currently, Message Queue for MQTT supports being accessed through port 1883 of the standard protocol. It also supports the SSL encryption, WebSocket, and Flash access methods. The endpoint URL is automatically allocated after an instance is created. Note down the URL for future reference. For information about how to create an MQTT instance, see [Quick start guide](../intl.en-US/Quick Start/Quick start guide.md#).

 ## MQTT-related terms {#section_bq5_obt_rhi .section}

 MQTT
 :   An industry standard protocol for the Internet of Things \(IoT\) and mobile Internet, which is suitable for data transmission between mobile terminals. By default, Message Queue for MQTT supports the MQTT protocol.

  Quality of service \(QoS\)
 :   An indication of the message transmission service quality. Possible QoS levels are:

-   QoS0, which indicates a maximum of one distribution.
-   QoS1, which indicates a minimum of one reaching.
-   QoS2, which indicates only one distribution.

  cleanSession
 :   A flag in MQTT that specifies whether the client is concerned about the previous status after a TCP connection is established. Its syntax is as follows:

-   cleanSession=true: When an offline client goes online again, it does not process any previous subscriptions or offline messages.
-   cleanSession=false: When an offline client goes online again, it always processes offline messages, while previous subscriptions remain effective.

 Note the following when using QoS and cleanSession in combination:

-   MQTT requires that cleanSession of each client be fixed during connection. Otherwise, offline messages may be incorrectly determined.
-   The external messages of MQTT that require QoS2 do not currently support non-cleanSession flags. If a client subscribes to messages that require QoS2, the subscription does not take effect even if the value of cleanSession is set to false.
-   cleanSession of P2P messages is determined by the client configuration of the sender.

[Table 1](#table_7gm_rsn_o4j) lists the results of different combinations of QoS and cleanSession:

|QoS level|cleanSession=true|cleanSession=false|
|---------|-----------------|------------------|
|0|No offline messages are processed, and online messages are pushed only once.|No offline messages are processed, and online messages are pushed only once.|
|1|No offline messages are processed, and online messages are guaranteed to be reached.|Offline messages are processed, and all messages are guaranteed to be reached.|
|2|No offline messages are processed, and online messages are guaranteed to be pushed only once.|Not supported|

## Solution-related terms {#section_8kc_1c6_8om .section}

 Real-Time Communication \(RTC\)
 :   A real-time network communication method for the voice and video fields. Currently, the mainstream use cases include voice calling, video calling, and video conferencing.

  RTC server
 :   This server hosts the audio/video and related media channel services provided by Alibaba Cloud RTC.

  RTC service management server
 :   A control node in the RTC system, which is also called the RTC management service. The RTC management service must be manually created, and controls the lifecycles of all RTC sessions. The management node is normally deployed in the cloud, and is constructed using Alibaba Cloud products.

  Audio/video mobile application
 :   A terminal application that is used by an end user in the RTC system. The end user uses the application to initiate or join an audio/video calls.

  Smart access point \(AP\)
 :   A common network device \(such as a smart router\) that supports application programming and can simultaneously handle Internet access and LAN device control functions.

  Digital price tags
 :   Electronic screens that are used in malls, supermarkets, and other places. They support networking based on wireless sensor network protocols \(such as Bluetooth and ZigBee\) as well as smart AP nodes for networking.

  Digital price tag management service
 :   The backend service of a digital price tag system. It is used to manage the content that is displayed on the electronic screens and to manage and query manual tasks, such as price changes.

  RDS
 :   A highly available and scalable online database service provided by Alibaba Cloud. It is used to make persistent the status changes of tasks \(such as price changes\) in a digital price tag system.

  Log Service
 :   The log storage service launched by Alibaba Cloud, which is used in a digital price tag system to persistently store all operation logs for auditing and tracing.

 
