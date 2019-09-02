# Scenario comparison {#concept_94521_zh .concept}

This topic describes the association and differences between MQ for MQTT and traditional MOM based on [EN-US\_TP\_152377.md\#](intl.en-US/Product Introduction/What is Message Queue for MQTT?.md#), and provides model selection recommendations in actual scenarios.

## Background {#section_fv5_3xn_hhb .section}

Traditional MOM, such as RocketMQ and Kafka, is intended for microservices and big data and implements message storage and forwarding. The message producer and consumer are applications for user services deployed on ECS instances.

Traditional MOM is applicable in scenarios where user services deployed on ECS instances adopt fixed technology stacks and language platforms. However, traditional MOM cannot deal with scenarios with access by massive multi-platform devices developed in multiple languages, where service properties play an important role during message production and consumption. Examples are the mobile Internet and IoT scenarios.

Designed based on the single-responsibility principle, MQ for MQTT is a stateless gateway intended for the mobile Internet and IoT, focusing only on the access, management, and message transmission of massive mobile devices. Messages to be stored are routed to backend storage services, such as RocketMQ and Kafka.

Based on such a division of responsibilities, MQ for MQTT routes the messages sent by devices to the bound storage service. Off-premises applications can still use traditional microservice development solutions and interact with terminal devices through an off-premises storage service. MQ for MQTT enables data exchange between off-premises applications and devices.

## Scenario comparison {#section_mpc_e04_516 .section}

A service scenario may include different types of application components, each of which plays a different role. Therefore, when selecting a message service, you need to understand the association and differences between MQ for MQTT and traditional MOM and use them in combination properly. For example, component A uses MQ for MQTT to send and receive messages, whereas component B uses RocketMQ for the same purpose.

The following describes the differences between MQ for MQTT and traditional MOM based on scenarios. RocketMQ is used as an example for comparison. Other message services, such as Kafka and AMQP \(RabbitMQ\), observe the same rules.

|Service|Scenarios|
|-------|---------|
|MQ for MQTT|MQ for MQTT is applicable in mobility scenarios with access by massive devices, each of which maintains a relatively small data volume. Therefore, MQ for MQTT can be used to process messages transmitted by a large number of online MQTT clients, each of which maintains a relatively small message volume. For example, many enterprises operate tens of thousands of and even millions of devices.|
|RocketMQ|RocketMQ is a messaging engine that is oriented towards user services deployed on ECS instances and mainly used for decoupling, asynchronous notification, and load shifting between service components. It is applicable in scenarios with a relatively small number of ECS instances that need to process massive messages and require high throughput. In general, only a few enterprises operate more than 10,000 ECS instances. Therefore, RocketMQ can be used to assist servers with massive data processing and analysis.|

## Scenarios of use of MQ for MQTT and RocketMQ together {#section_iif_1xl_ucz .section}

-   **Scenario 1**

    On the IoT, tens of thousands of and even millions of sensors can use MQ for MQTT to upload data, while applications for user services deployed on ECS instances can use RocketMQ to analyze and process the data.

-   **Scenario 2**

    On the Internet of Vehicles \(IoV\), you may need to upload vehicle information about millions of vehicles to the cloud \(ECS instances\), while the cloud delivers commands to any specific vehicles or broadcasts commands to all vehicles. Vehicles can connect to MQ for MQTT through the MQTT SDK for uploading data and receiving commands. Monitoring and management systems \(data analysis systems\) can use the RocketMQ SDK to subscribe to messages and deliver commands. The following figure shows the details.


![](images/43210_en-US.png "Scenario")

Based on the preceding differences, we recommend that you use MQ for MQTT for mobile devices and use RocketMQ \(or other message services\) for applications on user services that are deployed on ECS instances.

## Feature comparison {#section_ifs_6dn_cn3 .section}

The following table compares the features of MQ for MQTT and RocketMQ.

|Feature|MQ for MQTT|RocketMQ|
|-------|-----------|--------|
|Client connections|MQ for MQTT supports message processing for massive clients, which reach millions or tens of millions in quantity.|RocketMQ supports message processing for a relatively small number of servers, generally less than 10,000 in quantity.|
|Message volume per client|Each MQTT client processes a small number of messages, and sends and receives messages at regular intervals.|Each MQTT client processes a large number of messages and requires high throughput.|
|Deployment|Mobile devices, app software, and H5 pages|Server-side applications|
|Consumption mode|Broadcasting consumption|[Clustering consumption and broadcasting consumption](https://help.aliyun.com/document_detail/43163.html?spm=a2c4g.11174283.6.658.7369449cI7gOfB)|
|Sequence|Messages can be sent in order but cannot be received in order \(which will be available in the future\).|Messages can be sent and received in order.|
|Multi-language/system support \(TCP\)|Java, C, C++, . NET, Android, iOS, Python, JS, and Go|Java, C++, and . NET|
|Access credential|Permanent RAM access and on-demand access using an MQTT token. For more information, see [Authentication overview](../../../../intl.en-US/Authorization and Authentication/Authentication overview.md#).|[Permanent RAM access](https://help.aliyun.com/document_detail/61382.html?spm=a2c4g.11186623.6.637.2d0f313c48Je3R) and [on-demand STS-authorized access](https://help.aliyun.com/document_detail/93844.html?spm=a2c4g.11186623.6.638.8dcc7d46McdPx5)|

## Model selection instructions {#section_7x9_1pr_7ew .section}

The general principle of model selection is as follows:

-   RocketMQ is recommended for applications for user services deployed on ECS instances.

-   MQ for MQTT is recommended for applications that are deployed on mobile devices, apps, or web pages.


The following table lists the selection recommendations on MQ for MQTT and RocketMQ in common scenarios.

|Scenario|Deployment|MQ for MQTT|RocketMQ|
|--------|----------|-----------|--------|
|Status data reporting by devices|Mobile device|√|×|
|Receiving, processing, and analysis of device-reported data|Mobile device|×|√|
|Delivery of control commands to multiple devices|Server|×|√|
|Live broadcasting, bullet screen, chat apps that require sending and receiving messages|Application|√|×|
|Receiving and analysis of chat messages on the MQTT broker|Server|×|√|

**Note:** 

√ indicates that the MQ service is recommended, whereas × indicates that the MQ service is not recommended.

