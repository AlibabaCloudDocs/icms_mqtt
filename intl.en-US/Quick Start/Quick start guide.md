# Quick start guide {#concept_44867_zh .concept}

This topic provides the quick start guide on how to use Message Queue for MQTT to send and receive messages through the MQTT protocol that is supported by default.

If you want to access through a non-MQTT protocol, such as China New Energy Vehicle National Standards or China National GB-808 Standards, you must purchase Message Queue for MQTT Enterprise Platinum Edition. We provide the documentation and Customer Services for the Enterprise Platinum Edition in separate channels.

## Background {#section_a08_wel_m9l .section}

Message Queue for MQTT must be used with backend MQ.

-   An Message Queue for MQTT instance is a stateless gateway instance that is used to maintain client connections and to forward messages in mobile Internet and IoT scenarios. It does not support message data persistence. Therefore, you must configure a message storage instance for message storage and message data persistence.
-   Currently, each Message Queue for MQTT instance \(gateway instance\) must be bound to a storage instance \(RocketMQ instance\). Non-persistent usage \(in direct push mode, where messages are not persistent\) will be available in the future.
-   Currently, Message Queue for MQTT only supports RocketMQ instances for backend message storage. Message Queue for MQTT will support other types of storage instances in the future, such as Kafka and AMQP \(RabbitMQ\).
-   Currently, you can create a limited number of Message Queue for MQTT instances in a region, and one Message Queue for MQTT instance can be bound to only one RocketMQ instance. For the maximum number of instances that can be created in one region, see the console prompts.

When using Message Queue for MQTT, note the following network access restrictions:

Only the topics and group IDs on the same instance in the same region can be interconnected. For example, if a topic is created on Instance A in **China \(Beijing\)**, then the topic can be accessed only by the Message Queue for MQTT client \(hereinafter referred to as the client\) with the group ID that is created on Instance A in **China \(Beijing\)**.

## Process {#section_m4x_trp_hhb .section}

[Figure 1](#fig_dsf_ny4_hhb) shows how to send and receive messages through Message Queue for MQTT.

![Quick start flowchart](images/43300_en-US.png "Quick start process")

As shown in [Figure 1](#fig_dsf_ny4_hhb), you must create resources before sending and receiving messages on a client. Otherwise, the Message Queue for MQTT broker may deny the connections with invalid client IDs.

## Prerequisites {#section_mxv_grp_hhb .section}

-   You have activated the parent MQ service. If not, [activate](https://common-buy.aliyun.com/?spm=5176.ons.0.0.514e6c5c76HC2o&commodityCode=ons#/open) this service.

-   You have an Alibaba Cloud AccessKey. For more information, see [Create an AccessKey](../../../../../intl.en-US/General Reference/Create an AccessKey.md#).


## Step 1: Create resources {#section_kx4_3rp_hhb .section}

The resources include:

-   Message Queue for MQTT Instance \(for maintaining client connections and forwarding messages\)
-   Message storage instance \(for message storage, and only RocketMQ instances are currently supported\)
-   Topic \(level-1 topic for message sending and subscription, that is, the parent topic\)
-   Group ID \(for client identification\)

1.  **Select a region**.

    Determine the region in which the resources are to be created based on your service needs.

    1.  Log on to the [MQ console](http://ons.console.aliyun.com).

    2.  In the upper-left corner of the console, click the drop-down arrow on the right of the product name to switch **Message Queue** to **AliwareMQ for IoT**.

![Switch services](images/43307_en-US.png "Switch services")

    3.  In the top navigation bar, select a region in which you want to create the resources, such as **China \(Beijing\)**.

2.  **Create an Message Queue for MQTT instance**.

    First, you must create an Message Queue for MQTT instance. Note the following before creating an instance:

    -   You can create a limited number of instances in each region. For more information, see the console prompts.

    -   Estimate the TPS, number of connections, and number of subscriptions based on the service scenario. Select the appropriate instance specifications. For subscription instances, if you select an excessively small specification, rate limiting and throttling may be triggered and your services may be affected. Pay-As-You-Go instances also have a threshold that is set by default. If the threshold is exceeded, you need to submit a ticket for allocation. For the specific thresholds, see the default alarm levels of the instance.

    -   A Standard Edition instance takes effect upon purchase. An Enterprise Platinum Edition instance takes time to deploy and you will be notified when the instance is available.

    Follow these steps to create an Message Queue for MQTT instance:

    1.  In the left-side navigation pane, click **Overview**.

    2.  On the Instances page, click **Create Instance** in the upper-right corner.

    3.  In the **Create Instance** dialog box, select an Message Queue for MQTT instance version and go to the Purchase page. Select specifications as needed and complete the purchase as prompted.

    Return to the **Overview** page of the console, where you should be able to see the purchased \(created\) Message Queue for MQTT instance.

3.  **Create and bind a data storage instance**.

    After creating an Message Queue for MQTT instance, you must create an instance for storing topics and messages and bind the message storage instance to the Message Queue for MQTT instance. \(Currently, only RocketMQ instances are supported for data storage.\)

    The binding has the following limits:

    -   An Message Queue for MQTT instance can be bound only once, and its bound message storage instance cannot be changed once the binding is completed.

    -   Each storage instance can be bound to only one Message Queue for MQTT instance. One-to-multiple binding is not supported.

    -   The two instances to be bound must have the same namespace type. That is, an instance with an exclusive namespace cannot be bound to an instance with a non-exclusive namespace.

    -   If you delete the storage instance bound to an Message Queue for MQTT instance in advance, the Message Queue for MQTT instance may be unavailable.

    Follow these steps to create and bind a storage instance:

    1.  In the left-side navigation pane, click **Instances**. On the **Instances** tab page that appears by default, select the created Message Queue for MQTT instance.
    2.  In the **Step 2: Configure Message Storage** section, click the **RocketMQ \>\>** box.

        ![Bind an instance](images/47584_en-US.png "Bind an instance")

    3.  In the **Configure Message Storage** dialog box, set the parameters based on the instance and your requirements.
        -   If you have purchased a RocketMQ instance, select **Select Existing Instance**. A list of message storage instances that you have created \(purchased\) is displayed. Click an existing RocketMQ instance and then **OK** to complete the binding.

            ![Select an existing instance](images/47580_en-US.png "Select an existing instance")

        -   If you have not purchased a RocketMQ instance, do as follows:

            -   Click **Create Shared Instance** to create a RocketMQ Standard Edition instance. Enter an instance name and description. Then, click **OK**.

                ![Create a shared instance](images/47581_en-US.png "Create a shared instance")

            -   Click **Purchase Platinum Instance** to create a RocketMQ Platinum Edition instance. Click **Buy RocketMQ Platinum** and follow the prompts on the page to complete the purchase \(creation\).

                ![Create a Platinum Edition instance](images/47582_en-US.png "Create a Platinum Edition instance")

            After an instance is created, repeat [Step i](#li_6va_ldc_986) and [Step ii](#li_6yp_yg4_jom), select **Select Existing Instance**, click the created RocketMQ instance, and click **OK** to complete the binding.

4.  **Create a Topic**.

    To send and receive messages over MQTT, you must create an MQTT parent topic. Subtopics at different levels can be directly used in code without the need to create them.

    A one-to-one binding relationship is established between the Message Queue for MQTT instance and the storage instance. Therefore, the topic is actually created on the storage instance and mapped to the Message Queue for MQTT console. You can also perform all topic operations in the RocketMQ console.

    If you have already created a topic on the RocketMQ instance, you can use this topic directly. If you have not created any topics, perform the following steps:

    1.  In the left-side navigation pane, click **Message Storage**.

    2.  On the **Message Storage** page, select the created Message Queue for MQTT instance and click **Create Topic**.

![Create a topic](images/43326_en-US.png "Create a topic")

    3.  In the **Create Topic** dialog box, enter a topic name, select the message type of the topic for message storage, sending, and receiving, and enter remarks. Then, click **OK**.

    **Note:** To use an Message Queue for MQTT client to send ordered messages, select the ordered message type. Currently, Message Queue for MQTT clients do not support strongly ordered messages in consumption scenarios.

5.  **Create a group ID**.

    A group ID specifies the name of a group of nodes with identical logic and functions, representing a category of devices with the same functions. The group ID and device ID are used together to identify the client ID of an MQTT client. For more information, see [Terms](../intl.en-US/Product Introduction/Terms.md#).

    1.  In the left-side navigation pane, click **Groups**.

    2.  On the **Groups** page, select the created Message Queue for MQTT instance and click **Create Group ID**.

        ![Create a group ID](images/43327_en-US.png "Create a group ID")

    3.  In the **Create Group ID** dialog box, enter a group ID and click **OK**.

    The group ID appears on the **Groups** page after being created. The **Groups** page shows all your group IDs in the current region.

    **Note:** 

    -   Delete the group ID in a timely manner if it is no longer needed.

    -   A group ID can only be used by the account that created it. A group ID that is created by the primary account cannot be used by a sub-account. The sub-account must create its own group IDs separately.


## Step 2: Obtain an endpoint {#section_pyo_i1s_2xx .section}

To use an SDK to send and receive messages, you must use an endpoint to access the Message Queue for MQTT instance. An Message Queue for MQTT instance endpoint consists of the instance domain and port.

After the Message Queue for MQTT instance and the RocketMQ instance are bound, the endpoint information is immediately displayed in the **Endpoint Information** area.

You can also obtain the endpoint information by performing the following steps after binding the Message Queue for MQTT instance and the RocketMQ instance:

1.  In the top navigation bar of the console, select the region where the created resource is located, and then choose **Instances** in the left-side navigation pane.

2.  On the **Instances** tab page that appears by default, select the created Message Queue for MQTT instance and click the **Instance Information** tab.

3.  On the **Instance Information** tab page, view the domain name of the endpoint in the **Endpoint Information** area.

![Obtain the endpoint](images/43316_en-US.png "Obtain the endpoint")


**Public Endpoint** and **Intranet Endpoint** are available. We recommend that you use public endpoints for clients in IoT and mobile Internet scenarios. Intranet endpoints are for use only in some special scenarios. In general, we recommend that you use the server-side MQ products in cloud server scenarios, such as RocketMQ.

**Note:** To connect to the service with an endpoint on a client, use the domain name rather than the IP address because the IP address can change at any time. Alibaba Cloud is not responsible for any service interruptions caused by the direct use of IP addresses.

**Port**

Currently, Message Queue for MQTT supports MQTT on TCP, MQTT SSL, WebSocket, WebSocket SSL/TLS, and Flash. The corresponding service ports are listed in [Table 1](#table_vr8_2u1_b68). Replace the port number in an endpoint as required.

|MQTT on TCP|SSL|WebSocket|WebSocket SSL/TLS|Flash|
|-----------|---|---------|-----------------|-----|
|1883|8883|80|443|843|

## Step 3: Send and subscribe to messages by using an SDK {#section_umv_aus_nhl .section}

1.  Download the client SDK. For the download addresses of SDKs in different languages, see [Download the SDK](../intl.en-US/SDK Reference/Download the SDK.md#).

    Message Queue for MQTT supports the standard MQTT protocol by default, so we recommend that you use open source third-party client SDKs. If a client SDK in a desired language is not listed, search for MQTT-compatible SDKs on the Internet.

2.  Download the demo project and run the demo to send and subscribe to messages. For the download addresses of demo projects, see [Demo project](../intl.en-US/SDK Reference/Demo project.md#).

    The current demo library only covers some mainstream languages and will be updated later. If the corresponding development language is not covered, download a Java demo for modification. The demo project only demonstrates basic functions. You must modify all the parameters before using them in the actual online environment.


## More information {#section_c7u_uuk_ulw .section}

**Send messages in the console**

In addition to sending messages by using an SDK or API, you can send messages in the console to quickly verify the availability of the topic. The procedure is as follows:

1.  In the left-side navigation pane, click **Message Storage**.

2.  In the **Topics** list on the **Message Storage** page, locate the row that contains the created topic and click **Send** in the **Actions** column.

![Send messages](images/43328_en-US.png "Send messages")

3.  In the **Send Message** dialog box, set the message attributes, enter the message content, and click **OK**.

    The console returns a notification that the message has been sent successfully and the corresponding message ID.

    ![The message has been sent successfully.](images/43329_en-US.png "Sent successfully")


