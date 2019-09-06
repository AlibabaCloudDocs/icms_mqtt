# What is Message Queue for MQTT? {#concept_42419_zh .concept}

This topic describes the architecture, use cases, and benefits of Message Queue for MQTT. While traditional message queue \(MQ\) services are used between microservices, Message Queue for MQTT that is designed for the IoT implements message transmission between clients and the cloud to make the Internet of Everything a reality.

For information about how to use Message Queue for MQTT, see [Quick start guide](../intl.en-US/Quick Start/Quick start guide.md#).

## Architecture {#section_vjb_1lh_hhb .section}

Message Queue for MQTT is a lightweight, message-oriented middleware \(MOM\) launched by Alibaba Cloud for mobile Internet and IoT scenarios. Based on the features of message transmission in mobile Internet and IoT scenarios, it supports MQTT, STOMP, China National GB-808 Standards, China New Energy Vehicle National Standards, and other mainstream communication protocols. In addition, Message Queue for MQTT supports native TCP persistent connections, SSL encryption, WebSocket, and other transmission modes at the data link layer and supports mainstream development languages and platforms including C/C++, Java, iOS, and Android. [Figure 1](#fig_tny_xjh_hhb) shows the system technology stack of Message Queue for MQTT.

![](images/42260_en-US.png "Architecture")

## Scenarios {#section_ekb_f3h_hhb .section}

With the support for multiple protocols, languages, and platforms, Message Queue for MQTT is widely used in mobile Internet and IoT scenarios, including mobile live broadcasting, Internet of Vehicles, financial payments, smart catering, and instant chatting.

[Figure 2](#fig_yvq_4kh_hhb) shows the main use cases of Message Queue for MQTT.

![](images/42264_en-US.png "Scenarios")

## Benefits {#section_vwc_fbj_shb .section}

Message Queue for MQTT mainly provides mobile connection access, connection management, and data forwarding services. MQ for MQTT can be used with other Alibaba Cloud MQ services that support backend data persistence and message storage, such as traditional MOM \(MQ for RocketMQ and Kafka\). Also, it can serve as a connection gateway with unlimited scalability.Message Queue for MQTT is designed with a distributed architecture. With no single point of failure \(SPOF\) and infinite scalability between components, its architecture ensures that its capacity is completely transparent to you and can be adjusted according to your online usage.

[Figure 3](#fig_vnq_rkh_hhb) shows the benefits of Message Queue for MQTT.

![](images/42265_en-US.png "Benefits")

Message Queue for MQTT has the following advantages over other mobile message services:

-   It supports standard protocols, such as MQTT, STOMP, and China National GB-808 Standards. This means you are not bound to any technologies and can migrate the MQ service seamlessly to the cloud by using most open source SDKs.
-   As a persistent connection gateway that responds to massive mobile clients, its backend can communicate with other Alibaba Cloud MQ services. You can implement bidirectional communication between the client and the cloud without building individual gateways.
-   It supports device-level permission control and SSL/TLS-encrypted communication, making data transmission more secure and reliable.

