# Message structure mappings {#concept_112971_zh .concept}

MQ for MQTT is a gateway product that is intended for mobile devices. It is used for sending and receiving messages either independently or with other message storage services, such as MQ.

This topic describes the mappings between the message structures and properties involved in interaction with MQ for MQTT by using the MQ SDK, helping you better understand and use MQ for MQTT and MQ.

If you use MQ for MQTT independently, ignore the mappings described in this topic. Just observe the MQTT protocol.

For more information about MQ for MQTT, see [What is Message Queue for MQTT?](../../../../intl.en-US/Product Introduction/What is Message Queue for MQTT?.md#) and [Terms](../../../../intl.en-US/Product Introduction/Terms.md#).

## Message structure mappings {#section_b4f_3n4_hhb .section}

MQ for MQTT and MQ are messaging systems that are based on the publish-subscribe model and have similar concepts.

As shown in the preceding figure, MQ for MQTT supports multi-level topics, whereas MQ supports one-level topics. Therefore, a level-1 topic in MQ for MQTT is mapped to a topic in MQ, and level-2 and -3 topics in MQ for MQTT are mapped to the message properties in MQ.

The MQ protocol supports messages with custom properties, whereas the MQTT protocol does not support properties for the moment. However, part of the information in MQ for MQTT is mapped to message properties in MQ. This facilitates tracing of the headers and device information in the MQTT protocol and allows users of the MQ SDK to retrieve such information.

**Note:** For information about how to configure mappings from properties in MQ to information in MQ for MQTT, see the table in [Property mappings](#table).

MQ and MQ for MQTT use the data serialization results of your service messages as the payload, and do not perform further encoding and decoding of the service messages.

## Property mappings {#table .section}

The following table lists the property mappings currently supported between MQ for MQTT and MQ. Information can be set or retrieved by reading and writing these properties during interaction between applications that use the SDKs of MQ and MQ for MQTT.

For more information about QoS, cleanSession, topics, and client IDs, see [Terms](../../../../intl.en-US/Product Introduction/Terms.md#).

|Property key|Valid value|Description|
|------------|-----------|-----------|
|qoslevel|0, 1, and 2| This property can be set when MQ sends messages to MQ for MQTT. If it is not set, the default value 1 is used.

 It can be read directly from the messages that MQ for MQTT sends to MQ.

 |
|cleansessionflag|true and false| This property can be set when MQ sends P2P messages to MQTT clients. If it is not set, the default value "true" is used.

 This property is optional for other message types. It can be read directly from the messages that MQ for MQTT sends to MQ.

 |
|mqttSecondTopic|A string that indicates a specific subtopic| This property can be set when a subtopic is required to filter the messages that MQ sends to MQTT clients. If it is not set, the default value "null" is used.

 It can be read directly from the messages that MQ for MQTT sends to MQ.

 |
|mqttRealTopic|The sub-level string that services expect message-receiving clients to display| This property can be set when MQTT clients are expected to display the specified subtopic name after receiving messages from MQ. This property is typically applied to P2P messages. If it is not set, P2P messages use an invariable topic name by default.

 This property is unavailable for the messages that MQ for MQTT sends to MQ.

 |
|clientId|A string that indicates a specific client ID|This property cannot be set and is used to trace the ID of the MQTT client that sends a message to MQ.|

