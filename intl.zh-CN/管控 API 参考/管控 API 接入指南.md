# 管控 API 接入指南 {#concept_100742_zh .concept}

本文介绍微消息队列 for IoT 的 API 的接入方法和注意事项，包含 SDK 的获取以及初始化参数的设置。

## 1. SDK 获取 {#section_wvt_z24_hhb .section}

直接填写以下 pom.xml 的配置，依赖 API 的 SDK 即可。

```language-xml
<dependencies>
 		<dependency>
			<groupId>com.aliyun</groupId>
			<artifactId>aliyun-java-sdk-core</artifactId>
			<optional>true</optional>
			<version>4.3.3</version>
 		</dependency>
       <dependency>
           <groupId>com.aliyun</groupId>
           <artifactId>aliyun-java-sdk-ons</artifactId>
           <version>3.1.0</version>
       </dependency>
</dependencies>

```

## 2. 公共参数设置 { .section}

使用 API 前需启动 API 的客户端，而启动客户端需设置接入点、accessKey、secretKey 等参数信息，具体示例如下:

```language-java
    /**
    *API 的接入点，设置为目标 Region
    */
    String regionId = "XXXXX";
    /**
    *鉴权使用的 AccessKeyId，由阿里云官网管理控制台获取
    */
    String accessKey = "XXXXXXXXXXXXXXXXX";
    /**
    *鉴权使用的 AccessKeySecret，由阿里云官网管理控制台获取
    */
    String secretKey = "XXXXXXXXXXXXXXXXX";
    IClientProfile profile= DefaultProfile.getProfile(regionId，accessKey，secretKey);
    IAcsClient iAcsClient= new DefaultAcsClient(profile);
            

```

 **参数说明** 

启动客户端需设置的参数及其描述如下表所示。

|参数名称|描述|
|----|--|
|regionId|API 的网关所在地域（Region）|
|accessKey|您在阿里云服务器管理控制台上获取的 AccessKeyId|
|secretKey|您在阿里云服务器管理控制台上获得的 AccessKeySecret|

 **地域列表** 

微消息队列 for IoT 的 API 所支持的地域如下表所示。

|地域名称|RegionId|备注|
|----|--------|--|
|公共云华北2|cn-beijing|使用公共云华北2 地域的用户使用此接入点|
|公网|mq-internet-access|使用公网地域的用户使用此接入点|
|公共云华东1|cn-hangzhou|使用公共云华东1 地域的用户使用此接入点|
|公共云华东2|cn-shanghai|使用公共云华东2 地域的用户使用此接入点|
|公共云华南1|cn-shenzhen|使用公共云华南1 地域的用户使用此接入点|
|公共云新加坡|ap-southeast-1|使用新加坡地域的用户使用此接入点|
|金融云华东1|cn-hangzhou-finance|使用金融云华东1 地域的用户使用此接入点|
|金融云华南1|cn-shenzhen-finance-1|使用金融云华南1 地域的用户使用此接入点|
|金融云华东2|cn-shanghai-finance-1|使用金融云华东2 地域的用户使用此接入点|

