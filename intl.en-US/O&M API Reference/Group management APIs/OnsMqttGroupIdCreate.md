# OnsMqttGroupIdCreate {#concept_70067_zh .concept}

You can call this operation to create a group of MQ for MQTT, that is, a group ID, on the MQTT broker.

## Description {#section_qby_gd4_hhb .section}

This operation is for users of Enterprise Platinum Edition instances only. For more information, go to [Purchase of Enterprise Platinum Edition instances](https://common-buy-intl.aliyun.com/?commodityCode=onspre_intl#/buy).

**Scenarios**

When new applications are launched, if you need to use an MQTT client for sending and receiving messages with existing topics, you must create a group ID in the console or by calling this operation.

## Request parameters {#section_a2x_hd4_hhb .section}

|Name|Type|Required|Description|
|----|----|--------|-----------|
|OnsPlatform|String|No|The source of the request. By default, the request comes from the POP platform.|
|PreventCache|Long|Yes|The parameter that is used for CSRF verification, which can be set to the current system time. Unit: ms|
|GroupId|String|Yes|The ID of the MQTT group that you want to create.|
|InstanceId|String|Yes|The ID of the MQTT instance that corresponds to the group ID that you want to create.|

## Response parameters {#section_eht_3d4_hhb .section}

|Name|Type|Description|
|----|----|-----------|
|RequestId|String|A common parameter. Each request has a unique ID.|
|HelpUrl|String|A help link.|

## Example {#section_ncz_jd4_hhb .section}

The following example shows how to create a group ID named GID\_Test in the "cn-hangzhou" region.

``` {#codeblock_hjd_1t0_19b}
 public static void main(String []args) {
            String regionId = "cn-hangzhou";
            String accessKey = "XXXXXXXXXXXXXXXXX";
            String secretKey = "XXXXXXXXXXXXXXXXX";
            IClientProfile profile= DefaultProfile.getProfile(regionId，accessKey，secretKey);
            IAcsClient iAcsClient= new DefaultAcsClient(profile);
          OnsMqttGroupIdCreateRequest request = new OnsMqttGroupIdCreateRequest();
          request.setPreventCache(System.currentTimeMillis());
          request.setAcceptFormat(FormatType.JSON);
          request.setGroupId("GID_Test");
          request.setInstanceId("aaaaa");
          try {
              OnsMqttGroupIdCreateResponse response=iAcsClient.getAcsResponse(request);
              System.out.println(response.getRequestId());
          } catch (ServerException e) {
              e.printStackTrace();
          } catch (ClientException e) {
              e.printStackTrace();
          }
      }

			
```

