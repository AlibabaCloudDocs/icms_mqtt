# MQTT 与 RocketMQ 的消息结构映射 {#concept_112971_zh .concept}

本文针对使用消息队列 RocketMQ SDK 与微消息队列 MQTT 交互的场景，提供交互中所涉及的消息结构和属性字段的映射关系，方便您更好的理解和组合使用这两个产品。

 微消息队列 MQTT 是一款面向移动端的网关产品，在实际消息收发中，可单独使用，也可搭配其他存储产品，例如消息队列 RocketMQ 作为消息存储使用。

如果单独使用微消息队列 MQTT，则无需关注本文提供的映射关系，一切遵循标准 MQTT 协议规范即可。

关于微消息队列 MQTT 的详细介绍，请参见[什么是微消息队列 MQTT？](../intl.zh-CN/产品简介/什么是微消息队列 MQTT？.md#)和[名词解释](../intl.zh-CN/产品简介/名词解释.md#)。

## 消息结构映射 {#section_b4f_3n4_hhb .section}

 微消息队列 MQTT 和消息队列 RocketMQ 都是基于发布/订阅（Pub/Sub）模型的消息系统，两者概念上存在很多相似之处，下图列举了关键概念的区别和映射关系。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152389/156162719743276_zh-CN.png)

如上图所示，在微消息队列 MQTT 中 Topic 是多级结构，而消息队列 RocketMQ 的 Topic 仅有一级，因此，微消息队列 MQTT 中的一级 Topic 映射到消息队列 RocketMQ 的 Topic，而二级和三级 Topic 则映射到消息队列 RocketMQ 的消息属性（Properties）中。

消息队列 RocketMQ 协议中的消息（Message）可以拥有自定义属性（Properties），而 MQTT 协议目前的版本不支持属性，但为了方便溯源 MQTT 协议中的 Header 信息和设备信息，微消息队列 MQTT 的部分信息将被映射到消息队列 RocketMQ 的消息属性中，方便使用消息队列 RocketMQ 的 SDK 接入的用户获取。

**说明：** 关于具体如何在消息队列 RocketMQ 中设置属性字段来映射微消息队列 MQTT 信息，请参见下文[属性字段映射](#table)的表格。

消息队列 RocketMQ 和微消息队列 MQTT 的消息负载（Payload）均是您的业务消息的数据序列化结果，消息队列 RocketMQ 和微消息队列 MQTT 不会对业务消息再做进一步的编解码处理。

## 属性字段映射 {#table .section}

目前，微消息队列 MQTT 和消息队列 RocketMQ 支持的属性字段映射关系如下表所示。使用消息队列 RocketMQ 和微消息队列 MQTT 的 SDK 的应用交互时，可以通过读写这些属性字段来设置或获取信息。

关于 QoS、cleanSession、Topic 以及 Client ID 的详细解释，请参见[名词解释](../intl.zh-CN/产品简介/名词解释.md#)。

|属性 Key|属性可选值|说明|
|------|-----|--|
|qoslevel|0，1，2| 消息队列 RocketMQ 发给微消息队列 MQTT 消息时可以设置，如果不设置，默认为“1”；

  微消息队列 MQTT 发给消息队列 RocketMQ 的消息可以直接读取。

 |
|cleansessionflag|true，false| 消息队列 RocketMQ 发给微消息队列 MQTT 客户端 P2P 消息时设置，如不设置，默认为 “true”；

 其他消息不可以设置，微消息队列 MQTT 发给消息队列 RocketMQ 的消息可以直接读取。

 |
|mqttSecondTopic|具体的子级 Topic 字符串| 消息队列 RocketMQ 发给微消息队列 MQTT 客户端消息时如果需要子级 Topic 来做过滤，则设置，如不设置，默认为空；

  微消息队列 MQTT 发给消息队列 RocketMQ 的消息可以直接读取。

 |
|mqttRealTopic|业务上希望客户端收到消息时显示的子级字符串| 消息队列 RocketMQ 发给微消息队列 MQTT 客户端消息时如果希望客户端收到消息后显示成指定的子级 Topic 名称，则可以设置；一般用于 P2P 消息，若不设置，P2P 消息默认使用自己固定的 Topic；

  微消息队列 MQTT 发给消息队列 RocketMQ 的消息时无该属性。

 |
|clientId|具体的 clientId 字符串|不可设置，微消息队列 MQTT 发给消息队列 RocketMQ 消息时，用于追踪该消息的发送源的 clientId。|

