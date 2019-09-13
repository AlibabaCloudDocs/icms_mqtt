# P2P 消息收发模式（MQTT） {#concept_96176_zh .concept}

除了标准 MQTT 协议所支持的发布/订阅（Pub/Sub）消息收发模式外，微消息队列 MQTT 还支持点对点（Point to Point，简称 P2P）模式。本文介绍 P2P 模式的概念、原理以及如何使用微消息队列 MQTT 点对点收发消息。

## 什么是 P2P 模式 {#section_fg4_dka_kjz .section}

P2P，顾名思义，是一对一的消息收发模式，即只有一个消息发送者和一个消息接收者。而 Pub/Sub 模式通常用于一对多或多对多的消息群发场景，即拥有一个或多个消息发送者和多个消息接收者的场景。

在 P2P 模式中，发送者发送消息时已经明确该消息预期的接收者信息，并明确该消息只需要被特定的单个客户端消费。发送者发送消息时通过 Topic 信息直接指定接收者，接收者无需提前订阅即可获取该消息。

P2P 模式不仅可以为接收者节省注册订阅关系的成本，此外，由于收发消息的链路有单独的优化，还可以降低推送延迟。

## P2P 模式和 Pub/Sub 模式的区别 {#section_vnm_xnj_sgb .section}

在微消息队列 MQTT 中使用 P2P 模式收发消息与使用 Pub/Sub 的普通模式收发消息的区别如下所述：

-   发送消息时，Pub/Sub 模式下，发送者需要按照和接收者约定好的 Topic 发送消息；而 P2P 模式下，发送者无需事先约定传输消息的 Topic，发送者可以直接按照规范发送消息到目标的接收者。
-   接收消息时，Pub/Sub 模式下，接收者需要按照和发送者约定好的 Topic 提前订阅才能收到消息；而 P2P 模式下接收者无需事先订阅即可接收消息，从而简化接收者的程序逻辑，节省订阅成本。

## 发送 P2P 消息 {#section_amj_x54_54h .section}

使用 MQTT SDK 发送 P2P 消息时，需将二级 Topic 设为 “p2p”，将三级 Topic 设为目标接收者的 Client ID。

**Java 示例**

``` {#codeblock_5q3_eac_luv .language-java}
String p2pTopic =topic+"/p2p/GID_xxxx@@@DEVICEID_001";
sampleClient.publish(p2pTopic,message);
```

使用消息队列 MQ 的 SDK 发送 P2P 消息时，由于一级 Topic 和子级 Topic 是分开设置的，因此只需要将子级 Topic 属性设置成上述的子级 Topic 字符串。

**Java 示例**

``` {#codeblock_6dx_i0v_irv .language-java}
String subTopic="/p2p/GID_xxxx@@@DEVICEID_001";
msg.putUserProperties(PropertyKeyConst.MqttSecondTopic, subTopic);
```

[表 1](#table_f4u_8ag_p50) 提供了发送 P2P 消息的多语言代码示例的链接。

|语言|链接|
|--|--|
|.NET|[.NET 示例代码](https://github.com/AliwareMQ/lmq-demo/blob/master/lmq-DoNet-demo/MQTTSendP2PMessage.cs)|
|C|[C 示例代码](https://github.com/AliwareMQ/lmq-demo/blob/master/lmq-c-demo/src/c/mqttSendP2PMessageDemo.c)|
|Java|[Java 示例代码](https://github.com/AliwareMQ/lmq-demo/blob/master/lmq-java-demo/src/main/java/com/aliyun/openservices/lmq/example/demo/MQTTSendP2PMessage.java)|
|JavaScript|[JavaScript 示例代码](https://github.com/AliwareMQ/lmq-demo/blob/master/lmq-js-demo/mqttSendP2PMessage.html)|
|Python|[Python 示例代码](https://github.com/AliwareMQ/lmq-demo/blob/master/lmq-python-demo/MQTTSendP2PMessage.py)|
|PHP|[PHP 示例代码](https://github.com/AliwareMQ/lmq-demo/blob/master/lmq-php-demo/MQTTSendP2PMessageToMQTT.php)|

## 接收 P2P 消息 {#section_8qj_7me_syj .section}

接收消息的客户端无需任何订阅处理，只需要完成客户端的初始化即可收到 P2P 消息。

