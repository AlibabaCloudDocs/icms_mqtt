# Permission policies {#concept_2022293 .concept}

Alibaba Cloud offers Resource Access Management \(RAM\), which allows you to manage permissions for Message Queue for MQTT. With RAM, you can avoid sharing the key of your Alibaba Cloud account \(the AccessKey that contains AccessKey ID and AccessKey Secret\) with other users. Instead, you can grant them only the necessary permissions. This topic describes the permission policies of Message Queue for MQTT in RAM.

In RAM, a policy is a collection of permissions described by using [syntax structure](../../../../intl.en-US/Policy management/Policy language/Policy structure and grammar.md#). A policy can accurately describe the authorized resource sets, action sets, and authorization conditions. Message Queue for MQTT has the following two types of RAM policies:

-   [System policies](#section_q9r_783_p5l): Policies that are created by Alibaba Cloud. You can use but not modify these policies. Version updates of the policies are maintained by Alibaba Cloud.
-   [Custom policies](#section_di8_mdi_3gr): Policies that you can create, update, and delete. You also maintain the version updates of these policies.

## System policies {#section_q9r_783_p5l .section}

Message Queue for MQTT now provides three system policies by default.

**Note:** At present, Message Queue for MQTT does not support independent system policies. When you grant the following system permission policies to RAM users, they also apply to Message Queue in addition to Message Queue for MQTT.

|Policy name|Description|
|-----------|-----------|
|AliyunMQFullAccess|The permission to manage Message Queue for MQTT. It is the equivalent of the permission that the primary account has. A RAM user granted with this permission can send and receive all messages and use all functions of the console.|
|AliyunMQPubOnlyAccess|This permission provides the publish permission of Message Queue for MQTT. A RAM user granted with this permission can use all resources of the primary account to send messages through SDKs.|
|AliyunMQSubOnlyAccess|This permission provides the subscribe permission to Message Queue for MQTT. A RAM user granted with this permission can use all resources of the primary account to subscribe to messages through SDKs.|

## Custom policies {#section_di8_mdi_3gr .section}

With custom policies, you can grant fine-grained access to users.

**Mapping between resources and actions in Message Queue for MQTT**

In Message Queue for MQTT, instances, topics, and groups are different types of resources, and the permissions granted for these resources are actions. The naming formats of topics and groups vary depending on whether the instance has a namespace. You can check if an instance has a namespace on the **Instance Details** page in the MQTT console.

The following table lists mapping between resources and actions in Message Queue for MQTT.

|Resource|Naming format \(with namespace\)|Naming format \(no namespace\)|Action name|Action description|Note|
|--------|--------------------------------|------------------------------|-----------|------------------|----|
|Instance|acs:mq:\*:\*:\{mqttInstanceId\}|acs:mq:\*:\*:\{mqttInstanceId\}|mq:OnsMqttInstanceBaseInfo|Query the basic information of a specified instance.|Before granting permissions to a RAM user for topics and groups, you must grant the "mq: OnsMqttInstanceBaseInfo" permission of the instance to which the topics and groups belong.|
|mq:OnsMqttInstanceDelete|Delete an instance|None|
|mq:OnsMqttInstanceUpdate|Update instance information|None|
|mq:OnsMqttInstanceBind|Bind an instance|If you need to bind a Message Queue instance, you must have the permissions to operate on the specified Message Queue for MQTT instance and the bound Message Queue instance. For the permission settings of the Message Queue instances, see its permission policies.|
|mq:OnsMqttInstanceList|Obtain the list of instances|None|
|mq:OnsMqttInstanceUpdateWarn|Update the alerting information of a specified instance|None|
|Topic|acs:mq:\*:\*:\{storeInstanceId\}%\{topic\}|acs:mq:\*:\*:\{topic\}|mq:OnsMqttQueryClientByTopic|Query the clients that subscribed to a topic|Before granting permissions to a RAM user for topics and groups, you must grant the "mq:OnsMqttInstanceBaseInfo" permission of the instance to which the topics and groups belong. **Note:** 

-   If it is an instance with independent namespace, the topic must use the ID of the bound Message Queue for MQTT instance as the prefix.
-   The storeInstanceid here is the ID of the Message Queue instance that is bound to the Message Queue for MQTT instance. You can find the ID of the Message Queue instance on the **Instance Details** page in the MQTT console.

 |
|mq:OnsMqttQueryMsgTransTrend|Query statistics on sending and receiving messages by topic|
|mq:OnsMqttMessageSend|Test the function of sending messages in the MQTT console|
|Group ID|acs:mq:\*:\*:\{mqttInstanceId\}%\{groupId\}|acs:mq:\*:\*:\{groupId\}|mq:OnsMqttGroupIdCreate|Create a group ID|Before granting permissions to a RAM user for topics and groups, you must grant the "mq:OnsMqttInstanceBaseInfo" permission of the instance to which the topics and groups belong. **Note:** If it is an instance with independent namespace, the group ID must use the ID of the bound Message Queue for MQTT instance as the prefix.

 |
|mq:OnsMqttGroupIdList|Obtain the list of group IDs|
|mq:OnsMqttQueryClientByClientId|Query MQTT client information by client ID|
|mq:OnsMqttQueryClientByGroupId|Query MQTT client information by group ID|
|mq:OnsMqttQueryHistoryOnline|Query historical online MQTT client information by group ID|
|mq:OnsMqttGroupIdDelete|Delete a group ID|
|mq:OnsMqttDeviceTrace|Query MQTT device traces|
|mq:OnsMqttQueryClientTrace|Query messages related to an MQTT client|

**Examples of common policies**

To directly copy the sample code, delete the comments \("//" and the text description that follows\).

-   **Example 1: Grant permissions to query message statistics by a specific topic and query historical online connections by group ID in a specific instance.** 

-   For instances **with namespaces** 

    ``` {#codeblock_tru_z6q_tsg}
    {
        "Version":"1",
        "Statement":[
            {    // Grant permissions for an instance. Before granting permissions for topics and groups, you must grant permission for the corresponding instance (applicable to instances with namespaces)
                "Effect":"Allow",
                "Action":[
                    "mq:OnsMqttInstanceBaseInfo"
                ],
                "Resource":[
                    "acs:mq:*:*:{mqttInstanceId}"
                ]
            },
    {// Grant the permission to query statistics by topic
                "Effect":"Allow",
                "Action":[
                    "mq:OnsMqttQueryMsgTransTrend"
                ],
                "Resource":[
                    "acs:mq:*:*:{storeInstanceId}%{topic}"
                ]
            },
            {    // Grant permissions for a group
                "Effect":"Allow",
                "Action":[
                    "mq:OnsMqttQueryHistoryOnline"
                ],
                "Resource":[
                    "acs:mq:*:*:{mqttInstanceId}%{groupId}"
                ]
            }
        ]
     }
    ```

-   For instances **without namespaces** 

    ``` {#codeblock_3qa_mva_mq0}
    {
        "Version": "1", 
        "Statement": [
            {// Grant permissions for an instance. Before granting permissions for topics and groups, grant permissions for the corresponding instance (applicable to instances without namespaces)
                "Effect": "Allow", 
                "Action": [
                    "mq:OnsMqttInstanceBaseInfo"
                ], 
                "Resource": [
                    "acs:mq:*:*:{mqttInstanceId}"
                ]
            }, 
            {    // Grant the permission to query statistics by topic
                "Effect": "Allow", 
                "Action": [
                    "mq:OnsMqttQueryMsgTransTrend"
                ], 
                "Resource": [
                    "acs:mq:*:*:{topic}"
                ]
            }, 
            {// Grant permission for a group
                "Effect": "Allow", 
                "Action": [
                    "mq:OnsMqttQueryHistoryOnline"
                ], 
                "Resource": [
                    "acs:mq:*:*:{groupId}"
                ]
            }
        ]
    }
    ```

-   **Example 2: Grant permission for an entire instance \(only applicable to instances with namespaces\)**

    To grant the permission for the entire instance, which means the permission to operate on all the resources in the instance, set the policy as follows.

    ``` {#codeblock_cfb_nfo_66n}
    {   // Only applicable to instances with namespaces
        "Version": "1", 
        "Statement": [
            {
                "Effect": "Allow", 
                "Action": [
                    "mq:*"
                ], 
                "Resource": [
                    "acs:mq:*:*:{mqttInstanceId}*" //Grant permission for the instance. Replace {mqttInstanceId} with the ID of your instance.
                ]
            }
        ]
    }
    ```


## Next steps {#section_h3i_xzt_wm6 .section}

-   [Grant permissions to RAM users](intl.en-US/Access control/Grant permissions to RAM users.md#)

## More information {#section_0oq_jjy_t92 .section}

-   [Policy overview](../../../../intl.en-US/Policy management/Policy overview.md#)
-   [What is RAM?](../../../../intl.en-US/Product Introduction/What is RAM?.md#)
-   [Terms](../../../../intl.en-US/Product Introduction/Terms.md#)
-   [\(Optional\) Create a custom policy](../../../../intl.en-US/Quick Start/(Old Version) Quick Start/(Optional) Create a custom policy.md#)
-   [Create a RAM user](../../../../intl.en-US/Quick Start/(Old Version) Quick Start/Create a RAM user.md#)
-   [Authorize RAM users](../../../../intl.en-US/Quick Start/(Old Version) Quick Start/Authorize RAM users.md#)
-   [Use RAM to limit the IP addresses used to access Alibaba Cloud resources](../../../../intl.en-US/Tutorials/Use RAM to limit the IP addresses used to access Alibaba Cloud resources.md#)

