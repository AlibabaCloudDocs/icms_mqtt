# OnsMqttQueryMsgTransTrend {#concept_49232_zh .concept}

You can call this operation to query the volume of historical sent and received messages based on topics and other information.

## Description {#section_ez3_t14_hhb .section}

**Scenarios**

OnsMqttQueryMsgTransTrend is typically used to generate statistics and collect statistics on the service scale.

**Limits**

The MQTT broker performs throttling on frequent calls \(30 calls per minute\) because APIs are designed for custom management and control. Therefore, do not call this operation in the main service process. To determine whether devices are online, use the client online/offline notification function.

## Request parameters {#section_q3p_1b4_hhb .section}

|Name|Type|Required|Description|
|----|----|--------|-----------|
|OnsPlatform|String|No|The source of the request. By default, the request comes from the POP platform.|
|PreventCache|Long|Yes|The parameter that is used for CSRF verification, which can be set to the current system time. Unit: ms|
|ParentTopic|String|Yes|The parent topic that you want to query.|
|SubTopic|String|No|The subtopic that you want to query. This parameter can be empty if no subtopic exists or you want to query all the information about the parent topic.|
|MsgType|String|No|The message type. Valid values: -   P2P: point-to-point
-   SUB: publish-subscribe

 By default, all messages are queried if this parameter is not set. Other values are invalid.|
|TpsType|String|Yes|The query type. Valid values: -   TPS: queries the TPS of message production.
-   SUM: queries the total volume of produced messages.

 Other values are invalid.|
|TransType|String|Yes|The messaging type. Valid values: -   PUB: queries the total volume of produced messages at the producer.
-   SUB: queries the total volume of consumed messages at the consumer.

 Other values are invalid.|
|Qos|Integer|No|The QoS level of the query. Valid values: -   0: The request is distributed once at most.
-   1: The request is successfully distributed at least once.
-   2: The request is distributed only once.

 By default, all messages are queried if this parameter is set to other values or is not set. For more information about QoS, see [Terms](../../../../intl.en-US/Product Introduction/Terms.md#).|
|BeginTime|Long|Yes|The start time of the query.|
|EndTime|Long|Yes|The end time of the query. We recommend that the start time and end time of a query fall on the same day. Otherwise, the backend may truncate the query results.|
|InstanceId|String|Yes|The ID of the MQTT instance to which the queried messages belong.|

## Response parameters {#section_cbj_cb4_hhb .section}

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
|Y|Float|The Y axis, which represents the data \(TPS or total volume\).|

## Fields in Data {#section_4ck_qnq_k0i .section}

|Name|Type|Description|
|----|----|-----------|
|Title|String|The table name.|
|Records|List\(StatsDataDo\)|The information about a data collection point.|

## Fields in StatsDataDo {#section_wah_62l_zxt .section}

|Name|Type|Description|
|----|----|-----------|
|X|Long|The X axis, in the timestamp format. Unit: ms|
|Y|Float|The Y axis, which represents the data \(TPS or total volume\).|

## Example {#section_6ut_aoy_6op .section}

The following example is for reference only. It accesses the endpoint in the China \(Hangzhou\) region and shows how to query the number of messages under a specific topic over the past one hour.

``` {#codeblock_gpz_rgt_jin .language-java}


    public static void main(String[] args) {
        String regionId = "cn-hangzhou";
        String accessKey = "XXXXXXXXXXXXXXXXX";
        String secretKey = "XXXXXXXXXXXXXXXXX";
        IClientProfile profile= DefaultProfile.getProfile(regionId,accessKey,secretKey);
        IAcsClient iAcsClient= new DefaultAcsClient(profile);
        OnsMqttQueryMsgTransTrendRequest request = new OnsMqttQueryMsgTransTrendRequest();
        request.setInstanceId("aaaaa");
        request.setPreventCache(System.currentTimeMillis());
        request.setAcceptFormat(FormatType.JSON);
        request.setParentTopic("MQTT_TOPIC_ONSMONITOR_BJ");
        //request.setMsgType("sub");
        request.setTpsType("SUM");
        request.setTransType("PUB");
        //request.setQos(1);
        request.setBeginTime(System.currentTimeMillis()-1000*3600);
        request.setEndTime(System.currentTimeMillis());
        try {
                 OnsMqttQueryMsgTransTrendResponse response = iAcsClient.getAcsResponse(request);
                   OnsMqttQueryMsgTransTrendResponse.Data data =response.getData();
                System.out.println(data.getTitle()+"\n"+
                data.getRecords());
        } catch (ServerException e) {
          e.printStackTrace();
        } catch (ClientException e) {
          e.printStackTrace();
        }
    }

			
```

