# OnsMqttQueryClientByClientId {#concept_49228_zh .concept}

You can call this operation to query the connection information about the specified device based on a client ID, including the online information and subscriptions.

## Description {#section_i3y_k24_hhb .section}

**Scenarios**

OnsMqttQueryClientByClientId is typically used to track the running status of a single online device and to troubleshoot problems. Enter a client ID to check whether the corresponding device is online, and query the address and subscription information of the device.

For more information about client IDs, see [Terms](../../../../intl.en-US/Product Introduction/Terms.md#).

**Limits**

The MQTT broker performs throttling on frequent calls \(30 calls per minute\) because APIs are designed for custom management and control. Therefore, do not call this operation in the main service process. To determine whether devices are online, use the client online/offline notification function.

## Request parameters {#section_dnj_m24_hhb .section}

|Name|Type|Required|Description|
|----|----|--------|-----------|
|OnsPlatform|String|No|The source of the request. By default, the request comes from the POP platform.|
|PreventCache|Long|Yes|The parameter that is used for CSRF verification, which can be set to the current system time. Unit: ms|
|ClientId|String|Yes|The client ID that you want to query.|
|InstanceId|String|Yes|The ID of the MQTT instance that corresponds to the device that you want to query.|

## Response parameters {#section_y7x_hnu_cbc .section}

|Name|Type|Description|
|----|----|-----------|
|RequestId|String|A common parameter. Each request has a unique ID to facilitate troubleshooting and fault locating.|
|HelpUrl|String|A help link.|
|Data|MqttClientInfoDo|The data structure of device online information.|

**Fields in MqttClientInfoDo**

|Name|Type|Description|
|----|----|-----------|
|Online|Boolean|Indicates whether the device is online. Valid values: -   true: The device is online.
-   false: The device is offline.

 |
|ClientId|String|The client ID of the device.|
|SocketChannel|String|The IP address for device connection.|
|LastTouch|Long|The last update time.|
|SubScriptonData|List\(SubscriptionDo\)|The current subscription set of the device.|

**Fields in SubscriptionDo**

|Name|Type|Description|
|----|----|-----------|
|ParentTopic|String|The MQTT parent topic.|
|SubTopic|String|The MQTT subtopic. The parameter value is NULL if no subtopic exists.|
|Qos|Integer|The QoS level of the subscription. Valid values: -   0: The request is distributed once at most.
-   1: The request is successfully distributed at least once.
-   2: The request is distributed only once.

 For more information about QoS, see [Terms]().|

## Example {#section_w0m_1tx_mui .section}

The following example is for reference only. It accesses the endpoint of the China \(Hangzhou\) region and shows how to query the connection data about the device that corresponds to the specified client ID.

``` {#codeblock_cdt_uyh_j1s .language-java}


    public static void main(String[] args) {
        String regionId = "cn-hangzhou";
        String accessKey = "XXXXXXXXXXXXXXXXX";
        String secretKey = "XXXXXXXXXXXXXXXXX";
        IClientProfile profile= DefaultProfile.getProfile(regionId,accessKey,secretKey);
        IAcsClient iAcsClient= new DefaultAcsClient(profile);
        OnsMqttQueryClientByClientIdRequest request = new OnsMqttQueryClientByClientIdRequest();
        request.setPreventCache(System.currentTimeMillis());
        request.setAcceptFormat(FormatType.JSON);
        request.setClientId("GID_XXX@@@XXXXX");
        request.setInstanceId("aaaaa");
        try {
                 OnsMqttQueryClientByClientIdResponse response = iAcsClient.getAcsResponse(request);
                OnsMqttQueryClientByClientIdResponse.MqttClientInfoDo clientInfoDo = response.getMqttClientInfoDo();
                System.out.println(clientInfoDo.getOnline() + "  " +
                        clientInfoDo.getClientId() + "  " +
                        clientInfoDo.getLastTouch() + "  " +
                        clientInfoDo.getSocketChannel());
                for (OnsMqttQueryClientByClientIdResponse.MqttClientInfoDo.SubscriptionDo subscriptionDo : clientInfoDo.getSubScriptonData()) {
                    System.out.println(subscriptionDo.getParentTopic() + "   " + subscriptionDo.getSubTopic() + "  " + subscriptionDo.getQos());
                }
        } catch (ServerException e) {
          e.printStackTrace();
        } catch (ClientException e) {
          e.printStackTrace();
        }
    }

			
```

