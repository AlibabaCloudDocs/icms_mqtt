# Instance management {#concept_52975_zh .concept}

This topic describes how to renew, change the configuration of, and perform O&M on MQTT instances. Instance renewal and configuration change are only applicable to subscription instances, and are not applicable to Pay-As-You-Go instances.

## Renew an instance {#section_y43_c5h_hhb .section}

During the seven days before an MQTT instance expires, renewal notifications are sent to the mobile phone that is registered with the account. If you receive such notifications, renew your instance promptly. If the instance is not renewed upon expiration, it is retained for seven days and then is automatically released.

1.  In the left-side navigation pane, choose **Instances**.

2.  On the **Instances** page, select the target instance.

3.  On the **Instance Information** tab that appears by default, click **Renew**.


![](images/42289_en-US.png "Renew an instance")

**Activate automatic renewal**

You can configure automatic renewal for subscription instances in the Alibaba Cloud console to avoid impact to services resulting from instance expiration.

1.  On the top navigation bar, choose **Billing Management** \> **Renew** and go to the **Renew** console.

2.  In the left-side navigation pane, choose **MQTT**. Locate the row that contains the target instance and click **Auto Renew** in the **Actions** column.


## Change instance configuration {#section_inc_2ef_eev .section}

You need to scale out instances through configuration changes to handle the increasing service scale.

1.  In the left-side navigation pane, choose **Instances**.

2.  On the **Instances** page, select the target instance.

3.  On the **Instance Information** tab that appears by default, click **Upgrade**. The following figure shows the page that appears.

    **Note:** 

    -   The upgraded configuration of a Basic Edition instance takes effect in real time. The upgrade of an Enterprise Platinum Edition instance must be performed by a technical engineer of MQ for MQTT. The upgrade may take some time and does not affect service operation.

    -   Instance downgrading takes effect in real time. Note that refunds are unavailable for downgrades, so we recommend that you downgrade configuration near the end of a billing cycle.

    -   Cross-version downgrading is not supported. That is, the Basic Edition configuration cannot be changed to the Enterprise Platinum Edition, and vice versa.


## Perform instance O&M {#section_ix7_beb_ips .section}

You can view instance statistics during the instance runtime, including the message sending and receiving TPS, subscriptions, and client connections. You can set the instance alarm function to monitor instance usage in real time.

## View instance statistics {#section_rxy_2mt_dx3 .section}

1.  In the left-side navigation pane, choose **Instances**.

2.  On the **Instances** page, select the target instance.

3.  Click the **Statistics** tab and set the time range. Then, click **Search**.

    ![](images/42292_en-US.png "Instance O&M statistics")


## Set the instance monitoring alarm {#section_dgn_lk7_nh0 .section}

MQ for MQTT provides a series of metrics on a per-instance basis, such as message sending and receiving TPS, subscriptions, and client connections. You can configure the sending of alarm messages to the mobile phone numbers of contacts when these metrics exceed the alarm thresholds, to notify them to promptly upgrade specifications.

The alarm thresholds are set to 70% of the capacity by default. You can define the alarm thresholds and alarm switches.

1.  In the left-side navigation pane, choose **Instances**.

2.  On the **Instances** page, select the target instance.

3.  On the **Instance Information** tab that appears by default, click **Monitor**.

4.  In the **Monitoring Alarms** dialog box, enter an alarm threshold and select **Enable** for **Alarms**. Then, click **OK**.

    ![](images/42291_en-US.png "Monitoring Alarms")


