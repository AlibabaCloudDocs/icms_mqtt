# 创建 Group ID {#concept_70067_zh .concept}

## OnsMqttGroupIdCreate {#section_pby_gd4_hhb .section}

在服务器上创建微消息队列 MQTT 的分组，即 Group ID（GID）。

## 描述 {#section_qby_gd4_hhb .section}

本接口限企业铂金版用户专用，请前往[企业铂金版购买页](https://common-buy-intl.aliyun.com/?commodityCode=onspre_intl#/buy)查看详情。

**使用场景**

新应用上线时，在创建 Topic 资源后，如需使用 MQTT 客户端收发消息，首先需在控制台或者调用本接口创建 GID。

## 请求参数列表 {#section_a2x_hd4_hhb .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|OnsPlatform|String|否|该请求来源，默认来源是 POP 平台|
|PreventCache|Long|是|用于 CSRF 校验，设置为系统当前时间即可，单位毫秒（ms）|
|GroupId|String|是|创建的 MQTT 分组的 ID|
|InstanceId|String|是|需要创建 GID 的 MQTT 实例的 ID|

## 返回参数列表 {#section_eht_3d4_hhb .section}

|名称|类型|描述|
|--|--|--|
|RequestId|String|为公共参数，每个请求的 ID 都是唯一的|
|HelpUrl|String|帮助链接|

## 使用示例 {#section_ncz_jd4_hhb .section}

本示例在 cn-hangzhou 地域（Region）创建名为 GID\_Test 的 Group。

```
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

