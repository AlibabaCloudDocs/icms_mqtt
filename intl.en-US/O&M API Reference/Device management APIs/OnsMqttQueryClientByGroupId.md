# OnsMqttQueryClientByGroupId {#concept_49229_zh .concept}



##   {#section_ztt_g24_hhb .section}

You can call this operation to collect the number of the online devices in a group with the specified group ID.

## Description {#section_a5t_g24_hhb .section}

**Scenarios**

OnsMqttQueryClientByGroupId is typically used for service analysis and used to collect statistics on the activity level of the terminals that correspond to the specified group ID.

**Limits**

The MQTT broker performs throttling on frequent calls \(30 calls per minute\) because APIs are designed for custom management and control. Therefore, do not call this operation in the main service process.

## Request parameters {#section_dnn_h24_hhb .section}

|Name|Type|Required|Description|
|----|----|--------|-----------|
|OnsPlatform|String|No|The source of the request. By default, the request comes from the POP platform.|
|PreventCache|Long|Yes|The parameter that is used for CSRF verification, which can be set to the current system time. Unit: ms|
|GroupId|String|Yes|The group ID that you want to query.|
|InstanceId|String|Yes|The ID of the MQTT instance that corresponds to the group ID you want to query.|

## Response parameters { .section}

|Name|Type|Description|
|----|----|-----------|
|RequestId|String|A common parameter. Each request has a unique ID to facilitate troubleshooting and fault locating.|
|HelpUrl|String|A help link.|
|MqttClientSetDo|MqttClientSetDo|The data structure of online information about devices in the group.|

**Fields in MqttClientSetDo**

|Name|Type|Description|
|----|----|-----------|
|OnlineCount|Long|The total number of online devices in the group.|

## Example { .section}

The following example is for reference only. It accesses the endpoint of the China \(Hangzhou\) region and shows how to query the number of online devices that correspond to the specified group ID.

```language-java

    public static void main(String[] args) {
        String regionId = "cn-hangzhou";
        String accessKey = "XXXXXXXXXXXXXXXXX";
        String secretKey = "XXXXXXXXXXXXXXXXX";
        IClientProfile profile= DefaultProfile.getProfile(regionId,accessKey,secretKey);
        IAcsClient iAcsClient= new DefaultAcsClient(profile);
        OnsMqttQueryClientByGroupIdRequest request = new OnsMqttQueryClientByGroupIdRequest();
        request.setPreventCache(System.currentTimeMillis());
        request.setAcceptFormat(FormatType.JSON);
        request.setGroupId("GID_XXXXX");
		request.setInstanceId("aaaaa");
        try {
         	    OnsMqttQueryClientByGroupIdResponse response = iAcsClient.getAcsResponse(request);
                OnsMqttQueryClientByGroupIdResponse.MqttClientSetDo clientSetDo = response.getMqttClientSetDo();
                System.out.println(clientSetDo.getOnlineCount() + "  " +
                        clientSetDo.getPersistCount() + "  ");
        } catch (ServerException e) {
          e.printStackTrace();
        } catch (ClientException e) {
          e.printStackTrace();
        }
    }
    

```

