# OnsMqttQueryClientByTopic {#concept_49230_zh .concept}



##   {#section_h42_c24_hhb .section}

You can call this operation to collect the number of devices that subscribe to the specified MQTT topic.

## Description {#section_i42_c24_hhb .section}

**Scenarios**

OnsMqttQueryClientByTopic is typically used for service analysis. The devices that subscribe to a specific topic generally participate in the same activity. You can query the number of devices that participate in the activity.

**Limits**

The MQTT broker performs throttling on frequent calls \(30 calls per minute\) because APIs are designed for custom management and control. Therefore, do not call this operation in the main service process. To determine whether devices are online, use the client online/offline notification function.

## Request parameters {#section_pdk_d24_hhb .section}

|Name|Type|Required|Description|
|----|----|--------|-----------|
|OnsPlatform|String|No|The source of the request. By default, the request comes from the POP platform.|
|PreventCache|Long|Yes|The parameter that is used for CSRF verification, which can be set to the current system time. Unit: ms|
|ParentTopic|String|Yes|The parent topic that you want to query.|
|SubTopic|String|No|The subtopic that you want to query, which can be unspecified if there is no subtopic.|
|InstanceId|String|Yes|The ID of the MQTT instance that corresponds to the topic you want to query.|

## Response parameters { .section}

|Name|Type|Description|
|----|----|-----------|
|RequestId|String|A common parameter. Each request has a unique ID to facilitate troubleshooting and fault locating.|
|HelpUrl|String|A help link.|
|MqttClientSetDo| |The data structure of device online information.|

**Fields in MqttClientSetDo**

|Name|Type|Description|
|----|----|-----------|
|OnlineCount|Long|The total number of online devices that subscribe to the specified topic.|

## Example { .section}

The following example is for reference only. It accesses the endpoint of the China \(Hangzhou\)region and shows how to query the number of online devices that subscribe to the specified topic.

```language-java

    public static void main(String[] args) {
        String regionId = "cn-hangzhou";
        String accessKey = "XXXXXXXXXXXXXXXXX";
        String secretKey = "XXXXXXXXXXXXXXXXX";
        IClientProfile profile= DefaultProfile.getProfile(regionId,accessKey,secretKey);
        IAcsClient iAcsClient= new DefaultAcsClient(profile);
        OnsMqttQueryClientByTopicRequest request = new OnsMqttQueryClientByTopicRequest();
		request.setInstanceId("aaaaa");
        request.setPreventCache(System.currentTimeMillis());
        request.setAcceptFormat(FormatType.JSON);
        request.setParentTopic("XXXX");
        request.setSubTopic("/secTopic/0/1");//Leave this parameter unspecified if no subtopic exists. If a subtopic exists, enter complete information.
        try {
         	    OnsMqttQueryClientByTopicResponse response = iAcsClient.getAcsResponse(request);
                OnsMqttQueryClientByTopicResponse.MqttClientSetDo clientSetDo = response.getMqttClientSetDo();
                System.out.println(clientSetDo.getOnlineCount() + "  " +
                        clientSetDo.getPersistCount() + "  ");
        } catch (ServerException e) {
          e.printStackTrace();
        } catch (ClientException e) {
          e.printStackTrace();
        }
    }
    

```

