# Audio/video communication solution {#concept_227269 .concept}

The audio/video communication solution is co-developed by Alibaba Cloud AliwareMQ for IoT and Real-Time Communication \(Alibaba Cloud RTC\). It supports the rapid construction of products for a variety of real-time communication scenarios, such as online audio and video conferencing and one-to-one voice call applications. This topic describes the system architecture, data flow design, and related matters for attention in detail.

## Terms {#section_9jn_ht3_ayq .section}

 MQTT
 :   MQTT is a standard industry protocol for IoT and mobile Internet that is suitable for data transmission between mobile terminals.AliwareMQ for IoT supports this protocol, by default.

  MQTT broker
 :   AliwareMQ for IoT provides the MQTT broker that is used to interact with the MQTT protocol, and is used with the MQTT client and RocketMQ to send and receive messages.

  MQTT client
 :   The MQTT client is the node that is used to interact with the MQTT broker. In this solution, it specifically indicates the audio/video mobile application that sends or receives audio/video calling requests.

  P2P message
 :   AliwareMQ for IoT provides P2P messages based on the standard MQTT protocol. These messages are special messages because they can be directly sent to a specific target MQTT client without going through regular subscription relationship matching. For details, see [P2P messaging model](../intl.en-US/Function Overview/Advanced functions/P2P messaging model.md#).

  Real-time communication
 :   Real-time communication is a network communication method that is mainly used for the audio and video fields. Currently, the mainstream application scenarios include audio calling, video calling, and video conferencing.

  RTC server
 :   This server hosts the audio/video and related media channel services provided by Alibaba Cloud RTC.

  Control and management server of the audio/video service
 :   These servers are the control nodes in the audio/video communication system, which are referred to as audio/video management services in this topic. The audio/video management service is self-constructed, and is used to control the life cycle of all audio/video communication sessions. The management node is normally deployed on the cloud, and is constructed using Alibaba Cloud products.

  Audio/video mobile application
 :   This is the application on the terminal that is used by the end user in the audio/video communication system, which is referred to as the terminal application in this topic. The user uses this application to initiate or participate in audio/video calls.

 ## Solution architecture {#section_m6u_iuo_bph .section}

[Figure 1](#fig_r2l_7kh_ppy) shows the architecture of the audio/video communication solution.

![](images/46570_en-US.png "Solution architecture")

As shown in [Figure 1](#fig_r2l_7kh_ppy), the audio/video management service and the terminal application signal by using the AliwareMQ for RocketMQ, and implement service data interactions through Alibaba Cloud RTC. For more information, see [Data interaction](#section_n7z_mvf_kql).

## Advantages {#section_gtx_gou_qw9 .section}

The advantages of the audio/video communication solution are as follows:

-   Service capabilities are scalable.
    -   AliwareMQ for IoT and Alibaba Cloud RTC can be used on demand and dynamically scaled up to handle burst traffic peaks.
-   Network coverage is widespread.
    -   AliwareMQ for IoT and Alibaba Cloud RTC provide global deployment capabilities to achieve local service access and save cross-zone and cross-nation costs.
-   The construction period is short, supporting easy access.
    -   The construction process is O&M-free, reducing labor and hardware costs.
    -   The API is easy to use, supporting rapid implementation.
-   Security and reliability are high.
    -   All service nodes are highly available and stable.
    -   AliwareMQ for IoT supports SSL/TLS encryption and media streams support SRTP protection.

## Data interaction {#section_n7z_mvf_kql .section}

[Figure 2](#fig_a9b_5j3_c6u) shows the process of a real-time conference call based on AliwareMQ for IoT and Alibaba Cloud RTC. In this figure, the gray parts represent the self-built development programs or services, and the blue parts represent the services provided by AliwareMQ for IoT, RocketMQ, and Alibaba Cloud RTC.

![](images/46571_en-US.png "Data flow")

As shown in [Figure 2](#fig_a9b_5j3_c6u), User A invites User B to join an audio/video conference. The specific process is as follows:

1.  User A of the terminal application initiates a meeting request and sends the request to the MQTT broker by sending an MQTT message. The message is routed through AliwareMQ for IoT to the RocketMQ queue. The audio/video management service developed by the business side processes the meeting request by receiving the message. After verification, it calls the Alibaba Cloud RTC API to register the relevant resources and parameters of this communication.
2.  After receiving the parameters, the audio/video management service encapsulates the parameters into an invitation message and sends it to RocketMQ. After the message is routed through AliwareMQ for IoT, it is delivered to the terminal application of User A. Then, the terminal application of User A is added to the conference channel based on the parameters, and the access operation is completed.
3.  The audio/video management service also needs to find User B's information based on the information in User A's invitation. Similarly, the service also encapsulates the parameters into an invitation message, and the transfer process for User B is the same as that for User A described in [Step 2](#li_0xz_bzt_y85).
4.  The conference member User B joins the conference after receiving the parameters, and the communication initialization is completed.

Based on the preceding outline of the process, you can use AliwareMQ for IoT messages to perform other custom processes and operations, such as ending conferences, inviting others to join an ongoing conference, and muting certain users.AliwareMQ for IoT plays the signaling role in audio/video conferencing scenarios.

## Considerations {#section_mo6_v53_f1a .section}

The preceding process describes how to use AliwareMQ for IoT and Alibaba Cloud RTC to quickly build your own real-time communication application. For more information about the SDK, see the [AliwareMQ for IoT](https://www.alibabacloud.com/help/product/100973.htm) and [AliwareMQ for RocketMQ](https://www.alibabacloud.com/help/product/29530.htm) documents.

When using AliwareMQ for IoT to construct a signaling channel for real-time communication, follow these principles for message type and parameter design:

-   **Client ID mapping**

    The MQTT protocol requires that each client has a globally unique client ID. The client ID consists of two parts concatenated with the "@@@" separator. The final client ID must be unique and its length cannot exceed 64 characters. The two parts of the client ID are described as follows:

    -   Prefix group ID: Apply for the group ID on the AliwareMQ for IoT console. You are advised to classify the group IDs by application platform or channel to facilitate troubleshooting. For example, Android and iOS clients can be divided into different group IDs, or clients of different versions use different group IDs.
    -   Suffix device ID: The device ID is generated by the application. Device IDs can be mapped to application account IDs to ensure their global uniqueness.
    For more information about client IDs, see [Terms](../intl.en-US/Product Introduction/Terms.md#).

-   **Topic name mapping**

    To use AliwareMQ for IoT, you need to understand the MQTT subscription model. For more information, see the [protocol documentation](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html) and [official documentation](https://www.alibabacloud.com/help/product/100973.htm).

    MQTT is a message protocol that follows the publish/subscribe model. The subscription relationship and topic follow the directory tree format. Topics can be divided into parent topics and subtopics. The total length of a topic \(including parent topics and subtopics\) cannot exceed 64 characters. The types of topics are described as follows:

    -   Parent topic: The topic at the first level of the directory tree is a parent topic. The parent topic must be applied for on the AliwareMQ for IoT console before it can be used. After successful application, the parent topic is equivalent to a namespace.
    -   Subtopic: The parts of the topic subsequent to the first-level topic of the directory tree are referred to as the subtopics. You do not need to apply for a subtopic, and you can specify a subtopic as needed.
    For more information about topics, see [Terms](../intl.en-US/Product Introduction/Terms.md#).

    When designing a topic for sending and receiving messages, the business side must follow these principles:

    -   Different parent topics are used for upstream messages \(messages sent from the terminal application to the management service\) and downstream messages \(messages sent from the management service to the terminal application\).
    -   Messages with different priorities or large size differences use different parent topics.
    For the interaction process described above, you are advised to use P2P messaging provided by AliwareMQ for IoT. P2P messages do not need to be subscribed, allowing the producer to directly specify the peer consumer to receive them. For more information, see [P2P messaging model](../intl.en-US/Function Overview/Advanced functions/P2P messaging model.md#).

-   **Parameter design for message sending and receiving**

    Mobile applications may be killed in the background, making the mobile application offline. To handle this situation, you are advised to configure the terminal application as follows to ensure that the terminal application receives the previous message after it goes back online:

    -   Set the CleanSession parameter to "false".
    -   Set QoS to "1".
    The terminal application should perform deduplication and timeliness verification on the received messages \(this is applicable if the terminal application remains offline for more than one day and then receives the messages from the previous day when it goes online again\).

    For more information about CleanSession and QoS, see [Terms](../intl.en-US/Product Introduction/Terms.md#).


