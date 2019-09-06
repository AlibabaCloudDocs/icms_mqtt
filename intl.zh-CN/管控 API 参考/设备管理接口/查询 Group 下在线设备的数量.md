# 查询 Group 下在线设备的数量 {#concept_49229_zh .concept}

## OnsMqttQueryClientByGroupId {#section_ztt_g24_hhb .section}

根据 Group ID（GID）统计属于该 Group 下的在线设备的数量。

## 描述 {#section_a5t_g24_hhb .section}

**使用场景**

OnsMqttQueryClientByGroupId 接口一般用于业务分析，统计一个 GID 下终端设备的活跃程度。

**使用限制**

由于 OpenAPI 面向的场景是用户自定义管控开发，服务端会对过快的调用进行限流（每分钟 30 次），因此不要在业务的主流程中使用本接口。

## 请求参数列表 {#section_dnn_h24_hhb .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|OnsPlatform|String|否|请求来源，默认来源是 POP 平台|
|PreventCache|Long|是|用于 CSRF 校验，设置为系统当前时间即可，单位毫秒（ms）|
|GroupId|String|是|需查询的 GID|
|InstanceId|String|是|需查询的 GID 所属的 MQTT 实例 ID|

## 返回参数列表 { .section}

|名称|类型|描述|
|--|--|--|
|RequestId|String|为公共参数，每个请求的 ID 都是唯一的，可用于排查定位问题|
|HelpUrl|String|帮助链接|
|MqttClientSetDo|MqttClientSetDo|Group 下的设备在线信息数据结构|

**MqttClientSetDo 数据结构列表**

|名称|类型|描述|
|--|--|--|
|OnlineCount|Long|Group 所有在线设备数量|

## 使用示例 { .section}

本示例仅仅提供一个参考，从杭州接入点接入，查询指定 GID 的在线设备数量信息。

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

