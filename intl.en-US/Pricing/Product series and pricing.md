# Product series and pricing {#concept_52819_zh .concept}

This topic describes the instance types and billing models of MQ for MQTT. However, the prices and promotions on the [Purchase](https://common-buy.aliyun.com/?spm=a2c4g.11186623.2.23.7a0852detgpXCl&commodityCode=onsMqtt#/buy) page shall prevail.

 ![](images/43248_en-US.png "Product series")

## Instance types {#section_k6h_rno_s9h .section}

[Table 1](#table_ewj_cg4_hhb) describes the instance types and billing models of MQ for MQTT.

|Instance type|Instance model|Billing model|Scenarios|
|-------------|--------------|-------------|---------|
|Pay-As-You-Go instance|Pay-As-You-Go instances are shared services of MQ for MQTT, that is, their underlying hardware resources are shared. Multiple instances share the same backend cluster, and MQTT ensures service availability in multi-tenant scenarios.|The instances are billed in Pay-As-You-Go mode based on your actual usage.|Pay-As-You-Go instances are billed based on actual usage, so they are applicable to scenarios with large changes and fluctuations in business scale.|
|Basic Edition instance|Basic Edition instances are shared services of MQ for MQTT, that is, their underlying hardware resources are shared. Multiple instances share the same backend cluster, and MQTT ensures service availability in multi-tenant scenarios.|The instances are billed in subscription mode based on their specifications.|With low specifications and low prices, the Basic Edition instances are suitable for customers with small business scale.|
|Enterprise Platinum Edition instance|Enterprise Platinum Edition instances are exclusive services of MQ for MQTT, that is, the underlying hardware resources are exclusive. Each Enterprise Platinum Edition instance is deployed as an independent cluster, will not be affected by the business peaks of other users, and enjoys after-sales services and stability assurance with a higher priority.|The instances are billed in subscription mode based on their specifications.|With larger specifications and a higher cost, Enterprise Platinum Edition instances are suitable for users with large business scale and customization requirements.|

## Subscription billing {#section_w4i_7kd_qmu .section}

**1. Message sending and receiving TPS \(subscription\)**

**Definition**

Message sending and receiving TPS refers to the number of messages that are sent uplink and received downlink per second over protocols supported by MQ for MQTT, such as MQTT, China National GB-808 Standards, and China New Energy Vehicle National Standards.

**Note**

-   Message TPS refers to messages sent and received directly through MQ for MQTT, excluding messages directly sent and received through MQ.
-   The message TPS includes the TPS of received messages and the TPS of sent messages.
-   If messages with QoS = 1 and cleanSession = false are not pushed successfully, they are stored as offline messages for retrying. Offline message storage is also considered as one push call.
-   Each message is treated as a basic billing unit in the calculation of message receiving and sending TPS. Messages that require different transmission quality levels specified in a specific protocol are counted with a corresponding multiplication ratio in billing. For more information, see the following table.

|Transmission quality level|Calculation ratio|
|--------------------------|-----------------|
|None|1|
|QoS = 0 and cleanSession = true in MQTT|1|
|QoS = 0 and cleanSession = false in MQTT|1|
|QoS = 1 and cleanSession = true in MQTT|2|
|QoS = 1 and cleanSession = false in MQTT|5|
|QoS = 2 \(only cleanSession = true is supported\) in MQTT|5|

**Examples**

Assume that \[instance\_a\] has 100 MQTT clients, and cleanSession is set to true for each MQTT client. If one QoS0 message, two QoS1 messages, and three QoS2 messages are sent per second, and one QoS0 message, one QoS1 message, and one QoS2 message are received per second, the message TPS of \[instance\_a\] is:

100 x \(1 + 2 x 2 + 3 x 5\) + 100 x \(1 + 1 x 2 + 1 x 5\) = 2800

**2. Number of concurrent online connections**

**Definition**

The number of concurrent online connections is the number of client TCP connections on a single instance at any time. The maximum number of connections refers to the peak number of concurrent online connections, which is different from the number of daily and monthly active connections. The number of concurrent online connections is a transient value and is updated every minute.

**Note:** 

Tips: When purchasing an MQTT instance, select a reasonable connection quantity to avoid service throttling that is triggered by a peak connection pulse. Service throttling may cause client connection failures.

**Examples**

Assume that the number of concurrent online connections of \[instance\_a\] is 1000 at 10:00, and is 2000 at 10:01. The specifications of \[instance\_a\] must be greater than 2000 for proper service running.

**3. Number of subscription relationships**

**Definition**

The number of subscription relationships refers to the number of subscription rules that a user has registered with the MQTT broker.

**Note**

-   Each subscription to an MQTT topic by a client ID is counted as one subscription relationship.
-   The number of subscription relationships is counted every five minutes. The MQTT broker outputs the maximum value obtained in the statistical cycle.
-   According to the MQTT protocol, if cleanSession is set to true on an MQTT client, the MQTT broker cancels all its topic subscriptions after the MQTT client goes offline. If cleanSession is set to false, the MQTT broker retains its topic subscriptions.

**Examples**

Assume that \[instance\_a\] has two devices: client\_1 and client\_2. client\_1 subscribes to TopicA/sub\_1, TopicA/sub\_2, and TopicB. client\_2 subscribes to TopicA/sub\_1 and TopicB/sub\_2. The number of subscription relationships for \[instance\_a\] is 5 \(3 + 2 = 5\).

## Billing of Pay-As-You-Go instances {#section_vj6_jjv_nne .section}

**1. Number of sent and received messages**

**Definition**

The number of sent and received messages refers to the total number of messages that are sent uplink and received downlink over protocols supported by MQ for MQTT, such as MQTT, China National GB-808 Standards, and China New Energy Vehicle National Standards.

**Note**

-   The billing cycle is one day, that is, the number of messages in the 24 hours from 00:00 of the previous day is counted in the daily bill.
-   The number of sent and received messages counts only messages that are sent and received directly through MQ for MQTT, excluding messages directly sent and received through MQ.
-   If messages with QoS = 1 and cleanSession = false are not pushed successfully, they are stored as offline messages for retrying. Offline message storage is also considered as one push call.
-   Each message is treated as a basic billing unit in the calculation of the number of sent and received messages. Messages in different transmission quality levels specified in a specific protocol are counted with a corresponding multiplication ratio in billing.

|Transmission quality level|Calculation ratio|
|--------------------------|-----------------|
|None|1|
|QoS = 0 and cleanSession = true in MQTT|1|
|QoS = 0 and cleanSession = false in MQTT|1|
|QoS = 1 and cleanSession = true in MQTT|2|
|QoS = 1 and cleanSession = false in MQTT|5|
|QoS = 2 \(only cleanSession = true is supported\) in MQTT|5|

**Examples**

Assume that \[instance\_a\] has 100 MQTT clients, and cleanSession is set to true for each MQTT client. If one QoS0 message, two QoS1 messages, and three QoS2 messages are sent, and one QoS0 message, one QoS1 message, and one QoS2 message are received, the number of sent and received messages of \[instance\_a\] is:

100 x \(1 + 2 x 2 + 3 x 5\) + 100 x \(1 + 1 x 2 + 1 x 5\) = 2800

**2. Number of concurrent online connections**

**Definition**

The number of concurrent online connections refers to the number of client TCP connections on a single instance at any time.

**Note:** 

-   The billing cycle is one day, that is, the maximum number of concurrent online connections in the 24 hours from 00:00 in the previous day is counted in the daily bill.
-   For Pay-As-You-Go instances, the maximum number of concurrent online connections in the billing cycle is used, similar to the concept of daily active connections.

**Examples**

Assume that the number of concurrent online connections of \[instance\_a\] is 1000 at 10:00, 2017-08-08 and is 2000 at 11:00, 2017-08-08, and the number does not reach 2000 throughout the day, the maximum number of concurrent online connections of \[instance\_a\] is 2000. The number used for billing is 2000.

**3. Number of subscription relationships**

**Definition**

The number of subscription relationships refers to the number of subscription rules that a user has registered with the MQTT broker.

**Note:** 

-   The billing cycle is one day, that is, the maximum number of subscription relationships in the 24 hours from 00:00 of the previous day is counted in the daily bill.
-   For Pay-As-You-Go instances, the maximum number of subscription relationships in the billing cycle is used, similar to the concept of daily active connections.

**Examples**

Assume that the number of subscription relationships of \[instance\_a\] is 1000 at 10:00, 2017-08-08 and is 500 after deleting 500 subscription relationships at 11:00, 2017-08-08, and the number does not reach 1000 throughout the day, the maximum number of subscription relationships of \[instance\_a\] is 1000. The number used for billing is 1000.

