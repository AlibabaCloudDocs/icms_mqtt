# P2P messaging model {#concept_96176_zh .concept}

AliwareMQ for IoT supports the point-to-point \(P2P\) messaging model, in addition to the publish-subscribe messaging model that is supported by the standard MQTT protocol. This topic describes the concepts and principles of the P2P model as well as how to use AliwareMQ for IoT to send and receive messages through P2P.

## What is the P2P model? {#section_fg4_dka_kjz .section}

P2P, as its name implies, is a one-to-one messaging model where only one message sender and one message receiver are involved. The publish-subscribe model is usually used in one-to-many or many-to-many message transmission scenarios where one or more message senders and many message receivers are involved.

In the P2P model, the sender knows the information about the expected receiver when sending a message, and expects that the message will be consumed only by a specific client. The sender directly specifies the receiver in a topic when sending a message. The receiver can obtain the message without subscribing to the topic in advance.

The P2P model not only saves the subscription registration cost for the receiver but, due to the independently optimized messaging link, also reduces the push latency.

## Differences between the P2P model and the publish-subscribe model {#section_416_yw0_f10 .section}

The differences between the P2P model and the publish-subscribe model when used in AliwareMQ for IoT are as follows:

-   In the publish-subscribe model, the message sender needs to send the messages of the topic to which the receiver has subscribed. In the P2P model, the receiver does not need to subscribe to the topic, and the sender can send messages directly to the target client according to the standards.
-   In the publish-subscribe model, the receiver needs to subscribe to the topic in advance to receive messages from the sender. In the P2P model, the receiver does not need to subscribe to the topic in advance. Therefore, the program logic at the receiver is simplified and the subscription cost is saved.

## Send P2P messages {#section_wy1_znj_toj .section}

When using the MQTT SDK to send P2P messages, you must set the level-2 topic to "p2p" and the level-3 topic to the client ID of the target receiver.

**Java example**

``` {#codeblock_wkd_hk2_lxy .language-java}
String p2pTopic =topic+"/p2p/GID_xxxx@@@DEVICEID_001";
sampleClient.publish(p2pTopic,message);
```

When using the RocketMQ SDK to send P2P messages, you only need to set the subtopic attribute to the preceding subtopic string because the parent topic and subtopic are set separately.

**Java example**

``` {#codeblock_lca_cfe_mko .language-java}
String subTopic="/p2p/GID_xxxx@@@DEVICEID_001";
msg.putUserProperties(PropertyKeyConst.MqttSecondTopic, subTopic);
```

[Table 1](#table_f4u_8ag_p50) provides the links to multi-language sample codes for sending P2P messages.

|Language|Link|
|--------|----|
|.NET|[. NET sample code](https://github.com/AliwareMQ/lmq-demo/blob/master/lmq-DoNet-demo/MQTTSendP2PMessage.cs)|
|C|[C sample code](https://github.com/AliwareMQ/lmq-demo/blob/master/lmq-c-demo/src/c/mqttSendP2PMessageDemo.c)|
|Java|[Java sample code](https://github.com/AliwareMQ/lmq-demo/blob/master/lmq-java-demo/src/main/java/com/aliyun/openservices/lmq/example/demo/MQTTSendP2PMessage.java)|
|JavaScript|[JavaScript sample code](https://github.com/AliwareMQ/lmq-demo/blob/master/lmq-js-demo/mqttSendP2PMessage.html)|
|Python|[Python sample code](https://github.com/AliwareMQ/lmq-demo/blob/master/lmq-python-demo/MQTTSendP2PMessage.py)|
|PHP|[PHP sample code](https://github.com/AliwareMQ/lmq-demo/blob/master/lmq-php-demo/MQTTSendP2PMessageToMQTT.php)|

## Receive P2P messages {#section_z96_89z_h0u .section}

The client does not need to subscribe to P2P messages. Instead, it can receive P2P messages after initialization.

