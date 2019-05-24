# What is AliwareMQ for IoT? {#concept_42419_zh .concept}

This topic describes the architecture, scenarios, and benefits of AliwareMQ for IoT. While most traditional message queue \(MQ\) services are used among microservices, AliwareMQ for IoT implements message transmission between clients and clouds to make the interconnection of everything a reality.

For information about how to use AliwareMQ for IoT, see [Quick start guide](../../../../intl.en-US/Quick Start/Quick start guide.md#).

## Architecture {#section_vjb_1lh_hhb .section}

AliwareMQ for IoT is a lightweight message-oriented middleware \(MOM\) launched by Alibaba Cloud for mobile Internet and IoT scenarios. Based on the features of message transmission in mobile Internet and IoT scenarios, it supports MQTT, STOMP, China National GB-808 Standards, China New Energy Vehicle National Standards, and other mainstream communication protocols. In addition, AliwareMQ for IoT supports native TCP persistent connections, SSL encryption, WebSocket, and other transmission modes at the data link layer and supports mainstream development languages and platforms including C/C++, Java, iOS, and Android. The technology stack of the system is shown in [Architecture](#section_vjb_1lh_hhb):

 ![System stack](images/42260_en-US.png "Architecture") 

## Scenarios {#section_ekb_f3h_hhb .section}

With multi-protocol, multi-language, and multi-platform support capabilities, AliwareMQ for IoT is widely used in mobile Internet and IoT scenarios, including mobile live broadcasting, Internet of vehicles, financial payments, smart catering, instant chatting, and other scenarios.

[Figure 2](#fig_yvq_4kh_hhb) shows the main scenarios of AliwareMQ for IoT.

 ![](images/42264_en-US.png "Scenarios") 

## Benefits { .section}

AliwareMQ for IoT mainly provides mobile connection access, connection management, and data forwarding services. Used with other Alibaba Cloud MQ products that support backend data persistence and message storage, such as traditional MOM \(RocketMQ and Kafka\), AliwareMQ for IoT can serve as a connection gateway with unlimited scalability. AliwareMQ for IoT is designed with a distributed architecture. With no single point of failure \(SPOF\) and infinite scalability between components, its architecture ensures that its capacity is completely transparent to you and can be adjusted according to your online usage.

[Figure 3](#fig_vnq_rkh_hhb) shows the benefits of AliwareMQ for IoT.

![](images/42265_en-US.png "Benefits") 

Compared with other mobile messaging services, AliwareMQ for IoT has the following benefits:

-   It supports standard protocols, such as MQTT, STOMP, and China National GB-808 Standards. Therefore, you are not bound to any technologies and can migrate the MQ service seamlessly to the cloud by using most open source SDKs.
-   As a persistent connection gateway responding to massive mobile clients, its backend can communicate with other Alibaba Cloud MQ products. You can achieve bidirectional communication between the client and the cloud without building individual gateways.
-   It supports device-level permission control and SSL/TLS-encrypted communication, making data transmission more secure and reliable.

 

