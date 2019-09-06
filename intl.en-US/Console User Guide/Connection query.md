# Connection query {#concept_56902_zh .concept}

MQ for MQTT allows you to query information about online MQTT clients \(devices\). You can query the number of online devices that subscribe to a topic or correspond to a group ID, or query the online status and subscriptions of a device with the specified client ID.

You can go to the query page from one of the following paths:

-   In the left-side navigation pane, choose **Connections**.

-   In the left-side navigation pane, choose **Groups**. Locate the row that contains the target group ID and click **Connections** in the **Actions** column.


## Query by topic {#section_nyf_bwh_hhb .section}

You can query information about all the devices that subscribe to the specified topic.

1.  On the **Connections** page, click the **Search by Topic** tab.

2.  Enter complete topic information, including **Parent Topic** and **Subtopic**.

3.  Click **Search**.


## Query by group ID {#section_u34_tva_ht0 .section}

You can query the number of online devices that correspond to the specified group ID. This function is applicable to macro statistics.

1.  On the **Connections** page, click the **Search by Group ID** tab.

2.  Enter a group ID and set the query time range.

3.  Click **Search**.


The **Connected Clients** chart shows the number of online devices whose names start with the specified group ID in the selected time range. The statistics are collected by the distributed data collection component, which may result in delays and some errors.

## Query by client ID {#section_egp_hi6_0gz .section}

Each device has a unique client ID that is concatenated by the group ID and device ID. You can query the online status and subscriptions of the device by using the specified client ID.

1.  On the **Connections** page, click the **Search by Client ID** tab.

2.  Enter a group ID and device ID.

3.  Click **Search**.


The query result consists of two parts:

-   **Connection Information**: includes the client address and the last update time of the device.

-   **Subscription Information**: displays all the topics that are managed by the current client ID and the QoS information.


![](images/42302_en-US.png "Connection query")

