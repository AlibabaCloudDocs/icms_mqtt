# FAQ {#concept_44878_zh .concept}

**Q1: What should I do if I receive an alarm text message that indicates the use of an invalid group ID?**

**Cause:**

During access to MQ for MQTT, an MQTT client must first apply for a group ID in the console. If a group ID that has not been applied for is used, the MQTT client may be disconnected and become unable to send or receive messages.

**Suggestion:**

If the alarm text message notifies you of an invalid group ID, log on to the MQ for MQTT console and go to **Groups** to check whether the group ID has been created. If the group ID has not been created, create one manually. Ensure that the group ID complies with the naming rules.

**Q2: What should I do if I receive an alarm text message indicating that the number of stored offline messages exceeds the system limit?**

**Cause:**

MQ for MQTT limits the number of offline messages that can be stored in each instance. For more information about the storage limit, see [../../../../dita-oss-bucket/SP\_538/DNICMS19100486/EN-US\_TP\_152380.md\#](../../../../intl.en-US/SDK Reference/Limits.md#). If the number of offline messages exceeds the system limit due to an improper setting for client subscriptions, the system clears the offline messages \(starting from the earliest message\) until the message quantity falls below the limit.

**Suggestion:**

-   Based on the instance ID in the alarm message, check whether the MQTT clients that access the instance set QoS to 1 and cleanSession to false for message subscriptions.
-   Check whether most of the MQTT clients are offline.
-   Check whether messages are still sent to the offline MQTT clients.
-   If the problem is due to incorrect settings, set cleanSession to true and submit a ticket to clear incorrect data.

**Note:** 

cleanSession can be set to false during message subscription in the scenario where the client ID is unchanged each time when the MQTT client goes online and the MQTT client needs to obtain the messages that are not received during the offline period. Set cleanSession to true if the client ID changes each time when the MQTT client goes online or the client offline state is irrelevant to services. For more information, see [Terms](../../../../intl.en-US/Product Introduction/Terms.md#).

**Q3: What should I do if my MQTT client is disconnected abnormally?**

**Cause:**

-   The MQTT broker authenticates the MQTT client when the MQTT client sends the Publish and Subscribe messages. The MQTT client is disconnected if the authentication fails.
-   The MQTT clients that use the same client ID to access MQ for MQTT are disconnected forcibly.

**Suggestion:**

Ensure that each client ID is globally unique and that the MQTT client does not repeatedly connect to MQ for MQTT. In addition, prepare the reconnection logic.

**Q4: What should I do if the messages of a previously subscribed topic that is no longer required are still pushed?**

**Cause:**

Subscriptions in MQTT are persistent. Therefore, if you do not need to subscribe to a topic, call the unsubscribe method to cancel the subscription.

**Q5: Why are some topic messages not received?**

**Cause:**

Each MQTT client limits the number of subscriptions. For more information about the subscription limit, see [../../../../dita-oss-bucket/SP\_538/DNICMS19100486/EN-US\_TP\_152380.md\#](../../../../intl.en-US/SDK Reference/Limits.md#). If an MQTT client tries to subscribe to more topics than this limit, the messages of the topics that exceed the limit are dropped, and therefore cannot be received.

