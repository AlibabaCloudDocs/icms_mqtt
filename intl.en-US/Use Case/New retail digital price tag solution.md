# New retail digital price tag solution {#concept_227265 .concept}

The new retail digital price tag solution is launched by Alibaba Cloud AliwareMQ for IoT. It uses MQTT to manage data updates for digital price tags and multimedia screens in malls, supermarkets, and other public places. This topic takes digital price tags as an example to describe the system architecture, data flow design, and other key components of the solution in detail. Other similar industries can refer to this solution, making modifications as necessary for implementation.

## Terms {#section_il2_3kf_l3s .section}

 MQTT
 :   MQTT is a standard industry protocol for IoT and mobile Internet that is suitable for data transmission between mobile terminals.AliwareMQ for IoT supports this protocol, by default.

  MQTT broker
 :   AliwareMQ for IoT provides the MQTT broker that is used to interact with the MQTT protocol, and is used to receive and send messages.

  MQTT client
 :   The MQTT client is the node used to interact with the MQTT broker. In this solution, it specifically indicates a smart access point for sending or receiving price change messages.

  P2P message
 :   AliwareMQ for IoT provides P2P messages based on the standard MQTT protocol. These messages are special messages because they can be directly sent to a specific target MQTT client without going through regular subscription relationship matching. For details, see [P2P messaging model](../intl.en-US/Function Overview/Advanced functions/P2P messaging model.md#).

  Smart AP
 :   Smart access points \(APs\) are the smart routers and other network devices commonly seen on the market, which support application programming and can simultaneously handle Internet access and LAN device control.

  Digital price tag
 :   Digital price tags are distributed to digital displays in malls, supermarkets, and other places. Generally, they use wireless sensor network protocols such as Bluetooth and ZigBee as well as smart AP nodes for networking.

  Digital price tag management service
 :   In the digital price tag system, this backend service is used to manage the content displayed on the digital displays. This service primarily handles tasks that would otherwise be performed manually, like price changes.

  ApsaraDB for RDS
 :   This is a highly available and scalable online database service provided by Alibaba Cloud. It is used in the digital price tag system to preserve status changes for tasks such as price changes.

  Log Service \(SLS\)
 :   Alibaba Cloud's log storage service is used in the digital price tag system to persistently store all operation logs for auditing and tracing purposes.

 ## Solution architecture {#section_oyn_98m_yhc .section}

In the digital price tag solution, AliwareMQ for IoT is used in conjunction with multiple Alibaba Cloud products to implement update management for price tag data.[Figure 1](#fig_59g_5tp_zab) shows details on how the solution architecture of the digital price tag system works.

![](images/46288_en-US.png "Solution architecture")

As shown in [Figure 1](#fig_59g_5tp_zab), the digital price tag system primarily includes price tag nodes, smart AP nodes, AliwareMQ for IoT, RocketMQ, the backend control service for digital price tags, ApsaraDB for RDS, and Log Service. Each of these components is described as follows:

-   The smart AP forwards the status data of the price tag and receives price change commands. The smart AP uses the MQTT SDK to access Alibaba Cloud AliwareMQ for IoT over the public network based on the distribution of the stores or locations. This link uses SSL/TLS to encrypt transmissions, preventing data leaks.
-   One smart AP downlink and several price tag nodes can communicate with each other through a wireless sensor network such as Bluetooth or ZigBee.
-   The backend management service for digital price tags is deployed on the cloud through ECS. The RocketMQ SDK is used to interact with RocketMQ. The MQTT broker and the RocketMQ broker are natively interconnected.
-   The backend management service for digital price tags can persistently change the status in the RDS databases when price changes and other tasks are performed. It can store price tag report data and operations logs to Log Service to facilitate tracing and auditing.

## Advantages {#section_knr_f03_kur .section}

The advantages of the new retail digital price tag solution are as follows:

-   Powerful service capabilities that can be scaled automatically.
    -   AliwareMQ for IoT The message transmission capability is infinitely scalable, allowing you to increase the number of smart terminals without suffering from compromised system capabilities.
    -   AliwareMQ for IoT Information pushing in milliseconds is supported for millions of devices, with an even smaller latency for display updates of digital price tags.
-   Extensive application range, versatile generability, and rapid duplication.
    -   Based on the MQTT standard protocol, this solution is universally applicable. It can be replicated in similar scenarios by adapting the solution for different data content.
-   Security and reliability are high.
    -   AliwareMQ for IoT and smart AP nodes supports SSL/TLS encryption in data transmission, preventing media business data leakage.
    -   All service nodes are highly available and stable.

## Data interaction {#section_s0u_rpa_xwe .section}

**Status reporting**

1.  The digital price tag node uses a periodic polling mechanism to exchange data with the smart AP node, reporting its current display status, power capacity, and other information.
2.  The smart AP node organizes data and sends MQTT messages to the MQTT broker.
3.  The MQTT broker writes the reported message to the RocketMQ topic specified by the business side.
4.  The digital price tag management service uses the received RocketMQ messages from the queue to process and analyze the status of price tag nodes that are online in the current system. Then, it writes the data to Log Service.

**Update displays**

1.  The digital price tag management service triggers price change operations by sending RocketMQ messages.
2.  The MQTT broker routes the RocketMQ message, pushing the message to the target smart AP node by using the MQTT protocol.
3.  The smart AP node receives the price change notification and temporarily saves the task.
4.  The digital price tag node uses the polling mechanism to exchange data with the smart AP node and receives the new content to be displayed.
5.  After the target digital price tag node changes the price, the smart AP node returns an MQTT message to notify the digital price tag management service that the current task has been completed.
6.  The digital price tag management service writes the execution log of the current task to Service Log, facilitating subsequent tracing queries.

## Considerations {#section_hxp_0fz_jbx .section}

The preceding procedure describes how to use AliwareMQ for IoT and RocketMQ to build a digital price tag system. For more information about the SDK, see [MQTT](../intl.en-US/SDK Reference/Download the SDK.md#) and [RocketMQ](https://help.aliyun.com/document_detail/114448.html) documents.

When using AliwareMQ for IoT and RocketMQ to send commands, follow these principles for message type design and parameter design:

-   **SDK and protocol selection**

    In the digital price tag scenario, one application may be used by hundreds of offline stores. Generally, each store is equipped with several smart AP nodes. The number of smart AP nodes can be increased as the business scales out. This makes smart AP nodes suitable for access by using the MQTT protocol. The digital price tag management service is deployed on the cloud, making the use of cloud-based RocketMQ suitable for access.

-   **Client ID mapping**

    The MQTT protocol requires that each client has a globally unique client ID. The client ID consists of two parts concatenated with the "@@@" separator. The final client ID must be unique and its length cannot exceed 64 characters. The two parts of the client ID are described as follows:

    -   Prefix group ID: Apply for the group ID on the AliwareMQ for IoT console. Group IDs can be roughly classified by platform vendor or channel to facilitate troubleshooting. For example, different industries or batches can be divided into different group IDs, or clients of different versions can use different group IDs.
    -   Suffix device ID: The device ID is generated by the application. Device IDs can be encoded by using the unique information, such as the MAC address of the smart AP node.
    For more information about client IDs, see [Terms](../intl.en-US/Product Introduction/Terms.md#).

-   **Topic name mapping**

    To use AliwareMQ for IoT, you need to understand the MQTT subscription model. For more information, see the [protocol documentation](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html) and [official documentation](https://help.aliyun.com/product/100973.html).

    MQTT is a message protocol that follows the publishing/subscription model. The subscription relationship and topic follow the directory tree format. Topics can be divided into parent topics and subtopics. The total length of a topic \(including parent topics and subtopics\) cannot exceed 64 characters. The types of topics are described as follows:

    -   Parent topic: The topic at the first level of the directory tree is a parent topic. The parent topic must be applied for on the AliwareMQ for IoT console before it can be used. After successful application, the parent topic is equivalent to a namespace.
    -   Sub topic: The parts of the topic subsequent to the first-level topic of the directory tree are referred to as the subtopics. You do not need to apply for a subtopic, and you can specify a subtopic as needed.
    For more information about topics, see [Terms](../intl.en-US/Product Introduction/Terms.md#).

    When designing a topic for sending and receiving messages, the business side must follow these principles:

    -   Different types of tasks use different parent topics. For example, in this scenario, the price change tasks and the terminal status reporting tasks use different parent topics.
    -   In the digital price tag system, we recommend that you use P2P messaging provided by AliwareMQ for IoT for the interactive messages of price change tasks. P2P messages do not need to be subscribed, allowing the sender to directly specify the peer to receive them. For more information, see [P2P messaging model](../intl.en-US/Function Overview/Advanced functions/P2P messaging model.md#).
-   **Parameter design for message sending and receiving**

    Generally, price change tasks in the digital price tag scenario require real-time pushing. Therefore, we recommend that you configure the smart AP as follows during the interactions between the smart AP and the MQTT broker to ensure that the smart AP does not need to process the tasks that were pushed when it was disconnected:

    -   Set the CleanSession parameter to "true".
    -   Set QoS to "1".
    The smart AP should perform deduplication and timeliness verification on received messages.

    For more information about CleanSession and QoS, see [Terms](../intl.en-US/Product Introduction/Terms.md#).


