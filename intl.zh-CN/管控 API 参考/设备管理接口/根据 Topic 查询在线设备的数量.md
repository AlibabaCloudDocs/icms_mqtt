# 根据 Topic 查询在线设备的数量 {#concept_49230_zh .concept}

## OnsMqttQueryClientByTopic {#section_h42_c24_hhb .section}

根据 MQTT 的 Topic 统计订阅该 Topic 的在线设备的数量。

## 描述 {#section_i42_c24_hhb .section}

**使用场景**

OnsMqttQueryClientByTopic 接口一般用于业务分析。业务上订阅某个特定 Topic 的设备一般是参与某个特殊活动的设备，可以根据 Topic 查询参与该活动的设备数量。

**使用限制**

由于 OpenAPI 面向的场景是用户自定义管控开发，服务端会对过快的调用进行限流（每分钟 30 次），因此不要在业务的主流程中使用本接口。如需判断设备是否在线，请使用设备上下线通知功能。

## 请求参数列表 {#section_pdk_d24_hhb .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|OnsPlatform|String|否|请求来源，默认来源是 POP 平台|
|PreventCache|Long|是|用于 CSRF 校验，设置为系统当前时间即可，单位毫秒（ms）|
|ParentTopic|String|是|需查询的目标父 Topic|
|SubTopic|String|否|需查询的目标子 Topic，如果没有则不填|
|InstanceId|String|是|需查询的 Topic 所属的 MQTT 实例 ID|

## 返回参数列表 { .section}

|名称|类型|描述|
|--|--|--|
|RequestId|String|为公共参数，每个请求的 ID 都是唯一的，可用于排查定位问题|
|HelpUrl|String|帮助链接|
|MqttClientSetDo|MqttClientSetDo|设备在线信息数据结构|

**MqttClientSetDo 数据结构列表**

|名称|类型|描述|
|--|--|--|
|OnlineCount|Long|订阅指定 Topic 的所有在线设备数量|

## 使用示例 { .section}

本示例仅仅提供一个参考，从杭州接入点接入，查询订阅指定 Topic 的在线设备的数量。

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
        request.setSubTopic("/secTopic/0/1");//此处如果没有子 Topic 则不填；如果有，一定是完整的信息
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

