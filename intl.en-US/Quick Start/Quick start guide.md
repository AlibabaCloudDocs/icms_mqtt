# Quick start guide {#concept_44867_zh .concept}

This topic describes the complete process from creating instances and resources to sending and receiving messages, aiming to help you get started with AliwareMQ for IoT in a simple and clear way, use its functions, and familiarize yourself with its features.

AliwareMQ for IoT supports the MQTT protocol by default. Therefore, instructions in this topic are mainly based on MQTT.

If you want to access over a protocol other than MQTT, for example, China New Energy Vehicle National Standards or China National GB-808 Standards, you need to buy an Enterprise Platinum Edition instance. We deliver documentation and technical support for the Enterprise Platinum Edition in separate channels.

## Process {#section_m4x_trp_hhb .section}

[Figure 1](#fig_dsf_ny4_hhb) shows how to use AliwareMQ for IoT to send and receive messages.

 ![](images/43300_en-US.png "Quick start process")

As shown in the preceding figure, to use an MQTT client to send and receive messages, you must first purchase an MQTT instance and configure it, and create relevant resources required for sending and receiving messages, for example, topics and group IDs. The MQTT broker rejects connection requests from invalid client IDs.

## Prerequisites {#section_mxv_grp_hhb .section}

-   You have activated the parent MQ service. If not, [activate](https://common-buy.aliyun.com/?spm=5176.ons.0.0.514e6c5c76HC2o&commodityCode=ons#/open) this service.

-   You have an Alibaba Cloud AccessKey. If not, create an AccessKey by following the instructions provided in [Create an AccessKey](../../../../intl.en-US/General Reference/Create an AccessKey.md#). For more information about authentication, see [Authentication overview](../../../../intl.en-US/Authorization and Authentication/Authentication overview.md#).


## Step 1: Create resources {#section_kx4_3rp_hhb .section}

To use AliwareMQ for IoT, you must first create AliwareMQ for IoT resources, including the following:

-   MQTT instance \(used for client connection and message forwarding\)
-   Message storage instance \(for message storage. Only RocketMQ instances are supported currently.\)
-   Topic \(level-1 topic, that is, parent topic, for message sending and subscription\)
-   Group ID \(used for client identification\)

**Note:** 

-   An MQTT instance is a stateless gateway instance that is used to maintain client connection and forward messages in mobile Internet and IoT scenarios. It does not support message data persistence. Therefore, you must configure a message storage instance for message storage and message data persistence.
-   Currently, each MQTT instance \(gateway instance\) must be bound to a storage instance \(RocketMQ instance\). Non-persistent usage \(in direct push mode, where messages are not persistent\) will be available in the future.
-   Currently, AliwareMQ for IoT only supports RocketMQ instances for message storage. AliwareMQ for IoT will support other types of storage instances, such as Kafka and AMQP \(RabbitMQ\) in the future.
-   Currently, you can create a limited number of MQTT instances in one region, and each MQTT instance can be bound only to one RocketMQ instance. For the maximum number of instances that can be created in one region, see the console prompts.

**Select a region**

Determine the region in which the resources are to be created based on your business needs.

1.  Log on to the [Message Queue console](http://ons.console.aliyun.com).

2.  In the upper-left corner of the console, click the drop-down arrow on the right of the product name to switch **RocketMQ** to **AliwareMQ for IoT**.

3.  In the top navigation bar, select a region, for example, **Internet**, in which you want to create the resources.


**Create an MQTT instance**

First, you need to create an MQTT instance.

1.  In the left-side navigation pane, choose **Overview**.

2.  In the upper-right corner on the Instances page, click **Create Instance**.

3.  In the **Create Instance** dialog box, select the version of the MQTT instance and go to the Purchase page. Set the configurations as required and complete the purchase as prompted.


Go back to the **Overview** page in the console. You can find the purchased \(created\) MQTT instance.

**Note:** 

-   Each user can create a limited number of instances in each region. For more information, see the console prompts.

-   Estimate the TPS, number of connections, and number of subscription relationships based on the business scenario. Select a proper instance specification. For subscription instances, if you select an excessively small specification, service throttling may be triggered and your business may be affected. Pay-As-You-Go instances also have a threshold that is set by default. If the threshold is exceeded, you need to submit a ticket for allocation. For the specific thresholds, see the default alarm levels of the instance.

-   A Basic Edition instance takes effect upon purchase in real time. An Enterprise Platinum Edition instance takes time to deploy and you will be notified when the instance is available.


**Create and bind a data storage instance**

After creating an MQTT instance, you need to create an instance for storing topics and messages and bind the message storage instance to the MQTT instance. \(Currently, only RocketMQ instances are supported for data storage.\)

1.  In the left-side navigation pane, choose **Instances**. On the **Instances** tab page that appears by default, select the created MQTT instance.

2.  In the **Step 2: Configure Message Storage** area, click the **RocketMQ** box.

3.  In the **Message Storage Configuration** dialog box, configure the parameters based on the instance and your requirements:

    -   **Create Shared Instance**: Create a RocketMQ Basic Edition instance. Enter the instance name and description, and click **OK**. If this option is selected, repeat steps 1 and 2 after the RocketMQ instance is created, and select **Select Existing Instance** to complete the binding.

    -   **Purchase Platinum Instance**: Create a RocketMQ Enterprise Platinum Edition instance. Click **Buy RocketMQ Platinum** and follow the prompts on the page to complete the purchase.

4.  After the RocketMQ instance is created, go back to the **Message Storage Configuration** dialog box, select **Select Existing Instance**, and select the message storage instance that you have purchased and created. Click the RocketMQ instance that you have created and click OK to complete the binding.

    **Note:** 

    -   An MQTT instance can be bound only once, and its bounded message storage instance cannot be changed once the binding is complete.

    -   Each storage instance can only be bound to one MQTT instance. One-to-multiple binding is not allowed.

    -   The two instances to be bound must have the same namespace type, that is, an instance with an exclusive namespace cannot be bound to an instance with a non-exclusive namespace.

    -   If you delete a storage instance that is bound to an MQTT instance, the MQTT instance will be unavailable.


## Step 2: Obtain an endpoint { .section}

To use an SDK to send and receive messages, you must use an endpoint to access the service. An AliwareMQ for IoT endpoint consists of an endpoint domain name and a port.

After the MQTT instance and the RocketMQ instance are bound, the endpoint information appears immediately in the **Endpoint Information** area.

You can also obtain the endpoint information by following the subsequent steps after binding the MQTT instance and the RocketMQ instance:

1.  In the left-side navigation pane, choose **Instances**.

2.  On the **Instances** tab page that appears by default, select the created MQTT instance and click the **Instance Information** tab.

3.  On the **Instance Information** tab page, view the domain name of the endpoint in the **Endpoint Information** area.


AliwareMQ for IoT also provides **Public Endpoint** and **Intranet Endpoint**. We recommend that you use public endpoints for MQTT clients in IoT and mobile Internet scenarios. Intranet endpoints are for use only in some special scenarios. In general, we recommend that you use the server-side MQ products, for example, RocketMQ, in server scenarios.

**Note:** 

To connect to the service with an endpoint on an MQTT client, use the domain name rather than the IP address, because the IP address can change at any time. Alibaba Cloud is not responsible for service interruptions that are caused by direct use of IP addresses.

**Port**

Currently, AliwareMQ for IoT supports MQTT on TCP, MQTT SSL, WebSocket, WebScoket SSL/TLS, and Flash. The corresponding service ports are as follows. Replace the port number in an endpoint as required.

|MQTT on TCP|SSL|WebSocket|WebSocket SSL/TLS|Flash|
|-----------|---|---------|-----------------|-----|
|1883|8883|80|443|843|

## Step 3: Create a topic and a group ID { .section}

Note the following network access restrictions when using AliwareMQ for IoT:

MQTT clients of group IDs can access only topics of the same instance in the same region. That is, topics created in one instance in one region can only be accessed by consumers of group IDs created in the same instance in the same region.

For example, when a topic is created in instance A in **China \(Beijing\)**, it can only be accessed by consumers of group IDs created in instance A in **China \(Beijing\)**.

**Create a topic**

To send and receive messages over MQTT, you must create an MQTT parent topic. Subtopics at different levels can be directly used in the code without the need to create them in the console.

A one-to-one binding relationship is established between the MQTT instance and the storage instance. Therefore, the topic is in fact created on the storage instance and mapped to the AliwareMQ for IoT console. You can also perform all topic operations in the RocketMQ console. If you have already created a topic on the RocketMQ instance, you can use the topic directly. If you have never created any topics, perform the following steps:

1.  In the left-side navigation pane, choose **Message Storage**.

2.  On the **Message Storage** page, select the created MQTT instance, and click **Create Topic**.

3.  In the **Create Topic** dialog box, enter the topic name, select the message type of the topic for storage, sending, and receiving, and enter the remarks. Then, click **OK**.


**Note:** 

To use an MQTT client to send ordered messages, select the ordered message type. Currently MQTT clients do not support strongly ordered messages in consumption scenarios.

**Create a group ID**

The group ID in AliwareMQ for IoT specifies the name of a group of nodes with identical logic and functions, representing a category of devices with the same functions. The group ID and device ID are used together to identify the client ID of an MQTT client. For more information, see [Terms](../../../../intl.en-US/Product Introduction/Terms.md#).

1.  In the left-side navigation pane, choose **Groups**.

2.  On the **Groups** page, select the created MQTT instance, and click **Create Group ID**.

3.  In the **Create Group ID** dialog box, enter the group ID, and click **OK**.


The group ID appears on the **Groups** page after being created. The Groups page shows all group IDs that you own in the current region.

**Note:** 

-   Delete the group ID promptly if it is no longer required.

-   A group ID can only be used by the account that created it. A group ID that is created by the primary account cannot be used by a sub-account. The sub-account must create its own group IDs separately.


## Step 4: Send and subscribe to messages by using an SDK { .section}

1.  Download the MQTT client SDK. For the download addresses of SDKs in different languages, see [EN-US\_TP\_152385.md\#](intl.en-US/SDK Reference/Download the SDK.md#).

    **Note:** AliwareMQ for IoT supports the standard MQTT protocol by default, so we recommend that you use open-source third-party client SDKs. If a client SDK in a required language is not available in the list, search for MQTT-compatible SDKs on the Internet.

2.  Download the demo project and run the demo to send and subscribe to messages. For the download addresses of demo projects, see [Demo project](../../../../intl.en-US/SDK Reference/Demo project.md#).

    **Note:** 

    The current demo library only covers some mainstream languages and will be updated later. If the corresponding development language is not covered, download a Java demo for modification. The demo project only demonstrates basic functions. You must modify all the parameters before using it in an online environment.


**Send messages in the console**

In addition to sending messages by using an SDK or API, you can send messages in the console to quickly verify the availability of the topic. The procedure is as follows:

1.  In the left-side navigation pane, choose **Message Storage**.

2.  In the **Topics** list on the **Message Storage** page, locate the row that contains the topic that you have created and click **Send** in the **Actions** column.

3.  In the **Send Message** dialog box, set the message attributes, enter the message content, and click **OK**.

    The console returns a message sending success notification and the corresponding message ID.

     ![](images/43329_en-US.png "Message delivered")


