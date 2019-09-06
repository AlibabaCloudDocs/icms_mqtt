# 删除 Group ID {#concept_109584_zh .concept}

## OnsMqttGroupIdDelete {#section_uk5_wb4_hhb .section}

在服务器上删除已经存在的微消息队列 MQTT 的分组，即 Group ID（GID）。

## 描述 {#section_d5t_xb4_hhb .section}

本接口限企业铂金版用户专用，请前往[企业铂金版购买页](https://common-buy-intl.aliyun.com/?commodityCode=onspre_intl#/buy)查看详情。

**使用场景**

新应用迭代过程中，如果部分 GID 不再使用，可以在控制台或者调用本接口删除 GID。

## 请求参数列表 {#section_thl_zb4_hhb .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|OnsPlatform|String|否|该请求来源，默认来源是 POP 平台|
|PreventCache|Long|是|用于 CSRF 校验，设置为系统当前时间即可，单位毫秒（ms）|
|GroupId|String|是|需删除的 GID|
|InstanceId|String|是|需删除 GID 的 MQTT 实例的 ID|

## 返回参数列表 {#section_fvk_2c4_hhb .section}

|名称|类型|描述|
|--|--|--|
|RequestId|String|为公共参数，每个请求的 ID 都是唯一的|
|HelpUrl|String|帮助链接|

## 使用示例 {#section_t52_gc4_hhb .section}

本示例在 cn-hangzhou 地域（Region）删除名为 GID\_MingduanTest 的 GID。

```
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

