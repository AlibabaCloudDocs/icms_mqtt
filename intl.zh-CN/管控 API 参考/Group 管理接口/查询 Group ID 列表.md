# 查询 Group ID 列表 {#concept_49226_zh .concept}

## OnsMqttGroupIdList {#section_tbl_qc4_hhb .section}

查询目标地域（Region）里您所有的 Group ID（GID）。

## 描述 {#section_ubl_qc4_hhb .section}

OnsMqttGroupIdList 接口一般用于管理您所有的 GID 的场景。您可先获取 GID 列表，然后查询单个 GID 的相关数据。

## 请求参数列表 { .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|OnsPlatform|String|否|请求来源，默认来源是 POP 平台|
|PreventCache|Long|是|用于 CSRF 校验，设置为系统当前时间即可，单位毫秒（ms）|
|InstanceId|String|是|需查询的 GID 的 MQTT 实例 ID|

## 返回参数列表 { .section}

|名称|类型|描述|
|--|--|--|
|RequestId|String|为公共参数，每个请求的 ID 都是唯一的，可用于排查定位问题|
|HelpUrl|String|帮助链接|
|Data|MqttGroupIdDo|GID 信息数据结构|

**MqttGroupIdDo 数据结构列表**

|名称|类型|描述|
|--|--|--|
|Id|Long|数据编号|
|ChannelId|Integer|访问的途径编号，阿里云环境为“0”|
|Owner|String|GID 所属账号|
|GroupId|String|GID|
|Topic|String|该 GID 关联的父 Topic|
|Status|Integer|当前状态，取值说明如下： -   0：服务中
-   1：冻结
-   2：暂停

 |
|CreateTime|Long|创建时间，单位是毫秒时间戳|
|UpdateTime|Long|最后更新时间，单位是毫秒时间戳|
|InstanceId|String|当前 GID 所属的实例 ID|
|IndependentNaming|Boolean|当前 GID 所属实例是否有命名空间，取值说明如下： -   true：拥有独立命名空间，资源命名确保实例内唯一，跨实例之间可重名
-   false：无独立命名空间，实例内或者跨实例之间，资源命名必须全局唯一

 |

## 使用示例 {#section_uwx_vc4_hhb .section}

```
public static void main(String[] args) { String regionId = "cn-hangzhou"; String accessKey = "XXXXXXXXXXXXXXXXX"; String secretKey = "XXXXXXXXXXXXXXXXX"; IClientProfile profile= DefaultProfile.getProfile(regionId,accessKey,secretKey); IAcsClient iAcsClient= new DefaultAcsClient(profile); OnsMqttGroupIdListRequest request = new OnsMqttGroupIdListRequest(); request.setPreventCache(System.currentTimeMillis()); request.setAcceptFormat(FormatType.JSON); request.setInstanceId("aaaaa"); try { OnsMqttGroupIdListResponse response=iAcsClient.getAcsResponse(request); List groupIdList=response.getData(); for(OnsMqttGroupIdListResponse.MqttGroupIdDo groupDo:groupIdList){ System.out.println(groupDo.getId()+" "+ groupDo.getChannelId()+" "+ groupDo.getOwner()+" "+ groupDo.getGroupId()+" "+ groupDo.getTopic()+" "+ groupDo.getStatus()+" "+ groupDo.getCreateTime()+" "+ groupDo.getUpdateTime()); } } catch (ServerException e) { e.printStackTrace(); } catch (ClientException e) { e.printStackTrace(); } }
```

