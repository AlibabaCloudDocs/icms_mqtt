# OnsMqttGroupIdList {#concept_49226_zh .concept}



##   {#section_tbl_qc4_hhb .section}

You can call this operation to query all your group IDs in the target region.

## Description {#section_ubl_qc4_hhb .section}

OnsMqttGroupIdList is typically used to manage all your group IDs. You can obtain the group ID list and then query the data about a single group ID.

## Request parameters { .section}

|Name|Type|Required|Description|
|----|----|--------|-----------|
|OnsPlatform|String|No|The source of the request. By default, the request comes from the POP platform.|
|PreventCache|Long|Yes|The parameter that is used for CSRF verification, which can be set to the current system time. Unit: ms|
|InstanceId|String|Yes|The ID of the MQTT instance that corresponds to the group IDs you want to query.|

## Response parameters { .section}

|Name|Type|Description|
|----|----|-----------|
|RequestId|String|A common parameter. Each request has a unique ID to facilitate troubleshooting and fault locating.|
|HelpUrl|String|A help link.|
|Data|MqttGroupIdDo|The data structure of the group ID.|

**Fields in MqttGroupIdDo**

|Name|Type|Description|
|----|----|-----------|
|Id|Long|The data ID.|
|ChannelId|Integer|The access channel ID, which is 0 in Alibaba Cloud.|
|Owner|String|The account to which the group ID belongs.|
|GroupId|String|The group ID.|
|Topic|String|The parent topic associated with the group ID.|
|Status|Integer|The current status. Valid values: -   0: in service
-   1: frozen
-   2: suspended

 |
|CreateTime|Long|The creation time, in the timestamp format. Unit: ms|
|UpdateTime|Long|The last update time, in the timestamp format. Unit: ms|
|InstanceId|String|The ID of the instance to which the group ID belongs.|
|IndependentNaming|Boolean|Indicates whether the instance to which the group ID belongs has a namespace. Valid values: -   true: The instance has a separate namespace. A resource name must be unique within the same instance but can be duplicated across instances.
-   false: The instance has no separate namespace. Resource names must be unique within an instance and across instances.

 |

## Example {#section_uwx_vc4_hhb .section}

```
public static void main(String[] args) { String regionId = "cn-hangzhou"; String accessKey = "XXXXXXXXXXXXXXXXX"; String secretKey = "XXXXXXXXXXXXXXXXX"; IClientProfile profile= DefaultProfile.getProfile(regionId,accessKey,secretKey); IAcsClient iAcsClient= new DefaultAcsClient(profile); OnsMqttGroupIdListRequest request = new OnsMqttGroupIdListRequest(); request.setPreventCache(System.currentTimeMillis()); request.setAcceptFormat(FormatType.JSON); request.setInstanceId("aaaaa"); try { OnsMqttGroupIdListResponse response=iAcsClient.getAcsResponse(request); List groupIdList=response.getData(); for(OnsMqttGroupIdListResponse.MqttGroupIdDo groupDo:groupIdList){ System.out.println(groupDo.getId()+" "+ groupDo.getChannelId()+" "+ groupDo.getOwner()+" "+ groupDo.getGroupId()+" "+ groupDo.getTopic()+" "+ groupDo.getStatus()+" "+ groupDo.getCreateTime()+" "+ groupDo.getUpdateTime()); } } catch (ServerException e) { e.printStackTrace(); } catch (ClientException e) { e.printStackTrace(); } }
```

