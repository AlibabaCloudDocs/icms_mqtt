# OnsMqttGroupIdDelete {#concept_109584_zh .concept}

You can call this operation to delete an existing group of MQ for MQTT, that is, a group ID, from the MQTT broker.

## Description {#section_d5t_xb4_hhb .section}

This operation is for users of Enterprise Platinum Edition instances only. For more information, go to [Purchase of Enterprise Platinum Edition instances](https://common-buy-intl.aliyun.com/?commodityCode=onspre_intl#/buy).

**Scenarios**

You can delete the group IDs that are not used during new application iteration in the console or by calling this operation.

## Request parameters {#section_thl_zb4_hhb .section}

|Name|Type|Required|Description|
|----|----|--------|-----------|
|OnsPlatform|String|No|The source of the request. By default, the request comes from the POP platform.|
|PreventCache|Long|Yes|The parameter that is used for CSRF verification, which can be set to the current system time. Unit: ms|
|GroupId|String|Yes|The group ID that you want to delete.|
|InstanceId|String|Yes|The ID of the MQTT instance that corresponds to the group ID you want to delete.|

## Response parameters {#section_fvk_2c4_hhb .section}

|Name|Type|Description|
|----|----|-----------|
|RequestId|String|A common parameter. Each request has a unique ID.|
|HelpUrl|String|A help link.|

## Example {#section_t52_gc4_hhb .section}

The following example shows how to delete the group ID GID\_MingduanTest in the China \(Hangzhou\) region.

``` {#codeblock_ou6_ye8_hfc}
 public static void main(String []args) {
            String regionId = "cn-hangzhou";
            String accessKey = "XXXXXXXXXXXXXXXXX";
            String secretKey = "XXXXXXXXXXXXXXXXX";
            IClientProfile profile= DefaultProfile.getProfile(regionId，accessKey，secretKey);
            IAcsClient iAcsClient= new DefaultAcsClient(profile);
          OnsMqttGroupIdDeleteRequest request = new OnsMqttGroupIdDeleteRequest();
          request.setPreventCache(System.currentTimeMillis());
          request.setAcceptFormat(FormatType.JSON);
          request.setGroupId("GID_MingduanTest");
          request.setInstanceId("aaaaa");
          try {
              OnsMqttGroupIdDeleteResponse response=iAcsClient.getAcsResponse(request);
              System.out.println(response.getRequestId());
          } catch (ServerException e) {
              e.printStackTrace();
          } catch (ClientException e) {
              e.printStackTrace();
          }
      }

			
```

