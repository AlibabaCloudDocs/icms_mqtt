# Message structure mappings {#concept_112971_zh .concept}

AliwareMQ for IoT is a gateway product that is intended for mobile devices. It is used for sending and receiving messages either independently or with other message storage services, such as RocketMQ.

This topic describes the mappings between the message structures and properties involved in interaction with AliwareMQ for IoT by using the RocketMQ SDK, helping you better understand and use AliwareMQ for IoT and RocketMQ.

If you use AliwareMQ for IoT independently, ignore the mappings described in this topic. Just observe the MQTT protocol.

For more information about AliwareMQ for IoT, see [What is AliwareMQ for IoT?](../../../../intl.en-US/Product Introduction/What is AliwareMQ for IoT?.md#) and [Terms](../../../../intl.en-US/Product Introduction/Terms.md#).

## Message structure mappings {#section_b4f_3n4_hhb .section}

AliwareMQ for IoT and RocketMQ are messaging systems that are based on the publish-subscribe model and have similar concepts. The following figure shows the differences in the major concepts and mappings between AliwareMQ for IoT and RocketMQ.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152389/155868190243276_en-US.png)

As shown in the preceding figure, AliwareMQ for IoT supports multi-level topics, whereas RocketMQ supports one-level topics. Therefore, a level-1 topic in AliwareMQ for IoT is mapped to a topic in RocketMQ, and level-2 and -3 topics in AliwareMQ for IoT are mapped to the message properties in RocketMQ.

The RocketMQ protocol supports messages with custom properties, whereas the MQTT protocol does not support properties for the moment. However, part of the information in AliwareMQ for IoT is mapped to message properties in RocketMQ. This facilitates tracing of the headers and device information in the MQTT protocol and allows users of the RocketMQ SDK to retrieve such information.

**Note:** For information about how to configure mappings from properties in RocketMQ to information in AliwareMQ for IoT, see the table in [Property mappings](#table).

RocketMQ and AliwareMQ for IoT use the data serialization results of your service messages as the payload, and do not perform further encoding and decoding of the service messages.

## Property mappings {#table .section}

The following table lists the property mappings currently supported between AliwareMQ for IoT and RocketMQ. Information can be set or retrieved by reading and writing these properties during interaction between applications that use the SDKs of RocketMQ and AliwareMQ for IoT.

For more information about QoS, cleanSession, topics, and client IDs, see [Terms](../../../../intl.en-US/Product Introduction/Terms.md#).

|Property key|Valid value|Description|
|------------|-----------|-----------|
|qoslevel|0, 1, and 2| This property can be set when RocketMQ sends messages to AliwareMQ for IoT. If it is not set, the default value 1 is used.

 It can be read directly from the messages that AliwareMQ for IoT sends to RocketMQ.

 |
|cleansessionflag|true and false| This property can be set when RocketMQ sends P2P messages to MQTT clients. If it is not set, the default value "true" is used.

 This property is optional for other message types. It can be read directly from the messages that AliwareMQ for IoT sends to RocketMQ.

 |
|mqttSecondTopic|A string that indicates a specific subtopic| This property can be set when a subtopic is required to filter the messages that RocketMQ sends to MQTT clients. If it is not set, the default value "null" is used.

 It can be read directly from the messages that AliwareMQ for IoT sends to RocketMQ.

 |
|mqttRealTopic|The sub-level string that services expect message-receiving clients to display| This property can be set when MQTT clients are expected to display the specified subtopic name after receiving messages from RocketMQ. This property is typically applied to P2P messages. If it is not set, P2P messages use an invariable topic name by default.

 This property is unavailable for the messages that AliwareMQ for IoT sends to RocketMQ.

 |
|clientId|A string that indicates a specific client ID|This property cannot be set and is used to trace the ID of the MQTT client that sends a message to RocketMQ.|

