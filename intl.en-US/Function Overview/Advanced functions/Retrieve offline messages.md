# Retrieve offline messages {#concept_59914_zh .concept}

To simplify the offline message retrieving mechanism, AliwareMQ for IoT automatically loads offline messages and delivers them to an MQTT client after the MQTT client successfully establishes a connection and passes permission verification.

You must note the following:

-   After the MQTT client establishes a connection, it must pass the permission verification to automatically load offline messages. For example, if you use token verification for an MQTT client, you must upload the token and the MQTT client must pass the verification before it can receive offline messages.

-   An offline message takes some time to generate, because the pushed message can be judged as an offline message after the MQTT client acknowledgement times out. Therefore, if the MQTT client experiences transient disconnection and reconnection, you may not be able to obtain the offline notification message immediately. The latency is generally 5 to 10 seconds.

-   If you have too many offline messages, that is, more than 30 messages, AliwareMQ for IoT sends offline messages in batches \(30 messages a batch every 5 seconds\).


**Note:** 

The automatic loading mechanism allows you to replace the original active pulling method for retrieving offline messages, though you can still use the original method.

