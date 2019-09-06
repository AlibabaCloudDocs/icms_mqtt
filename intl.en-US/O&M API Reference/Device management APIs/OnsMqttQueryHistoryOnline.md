# OnsMqttQueryHistoryOnline {#concept_49231_zh .concept}



##   {#section_kt5_4d4_hhb .section}

You can call this operation to query the curve that shows the number of historical online MQTT clients \(devices\) by setting the time period and group ID.

## Description {#section_lt5_4d4_hhb .section}

**Scenarios**

OnsMqttQueryHistoryOnline is typically used to generate statistics.

**Limits**

The MQTT broker performs throttling on frequent calls \(30 calls per minute\) because APIs are designed for custom management and control. Therefore, do not call this operation in the main service process. To determine whether devices are online, use the client online/offline notification function.

## Request parameters {#section_qkx_pd4_hhb .section}

|Name|Type|Required|Description|
|----|----|--------|-----------|
|OnsRegionId|String|Yes|The region where the operated MQTT instance is located. For more information about supported regions, see [Guide on API access](intl.en-US/O&M API Reference/API access guide.md#).|
|OnsPlatform|String|No|The source of the request. By default, the request comes from the POP platform.|
|PreventCache|Long|Yes|The parameter that is used for CSRF verification, which can be set to the current system time. Unit: ms|
|GroupId|String|Yes|The group ID that you want to query.|
|BeginTime|Long|Yes|The start time of the query.|
|EndTime|Long|Yes|The end time of the query. We recommend that the start time and end time of a query fall on the same day. Otherwise, the backend may truncate the query results.|
|InstanceId|String|Yes|The ID of the MQTT instance that corresponds to the group ID you want to query.|

## Response parameters { .section}

|Name|Type|Description|
|----|----|-----------|
|RequestId|String|A common parameter. Each request has a unique ID.|
|HelpUrl|String|A help link.|
|data|Data|The data collection.|

**Fields in Data**

|Name|Type|Description|
|----|----|-----------|
|Title|String|The table name.|
|Records|List\(StatsDataDo\)|The information about a data collection point.|

**Fields in StatsDataDo**

|Name|Type|Description|
|----|----|-----------|
|X|Long|The X axis, in the timestamp format. Unit: ms|
|Y|Float|The Y axis, which represents the quantity.|

## Example { .section}

The following example is for reference only. It accesses the endpoint of the China \(Hangzhou\) region and shows how to query the number of online devices over the past one hour based on the specified group ID.

```language-java

    public static void main(String[] args) {
        String regionId = "cn-hangzhou";
        String accessKey = "XXXXXXXXXXXXXXXXX";
        String secretKey = "XXXXXXXXXXXXXXXXX";
        IClientProfile profile= DefaultProfile.getProfile(regionId,accessKey,secretKey);
        IAcsClient iAcsClient= new DefaultAcsClient(profile);
        OnsMqttQueryHistoryOnlineRequest request = new OnsMqttQueryHistoryOnlineRequest();
        request.setPreventCache(System.currentTimeMillis());
        request.setAcceptFormat(FormatType.JSON);
        request.setGroupId("XXX");
        request.setBeginTime(System.currentTimeMillis()-1000*3600);
        request.setEndTime(System.currentTimeMillis());
		request.setInstanceId("aaaaa");
        try {
         	    OnsMqttQueryHistoryOnlineResponse response = iAcsClient.getAcsResponse(request);
               	OnsMqttQueryHistoryOnlineResponse.Data data =response.getData();
                System.out.println(data.getTitle()+"\n"+
                data.getRecords());
        } catch (ServerException e) {
          e.printStackTrace();
        } catch (ClientException e) {
          e.printStackTrace();
        }
    }
    

```

