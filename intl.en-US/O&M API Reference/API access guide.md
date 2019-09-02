# API access guide {#concept_100742_zh .concept}

This topic describes the access method for the MQ for MQTT API and precautions, including how to obtain the SDK and set initialization parameters.

## 1. Obtain the SDK {#section_wvt_z24_hhb .section}

Enter the following pom.xml configuration to establish API dependency on the SDK.

``` {#codeblock_lki_3r1_0bo .language-xml}
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

## 2. Set common parameters {#section_hw1_8ds_qk8 .section}

Before using the API, start the API client by setting the endpoint, AccessKeyId, AccessKeySecret, and other parameters, as shown in the following example:

``` {#codeblock_e4c_pcn_t8v .language-java}
    /**
    * The endpoint of the API, which is set to the target region.
    */
    String regionId = "XXXXX";
    /**
    * The AccessKeyId that is created in the Alibaba Cloud console for identity authentication.
    */
    String accessKey = "XXXXXXXXXXXXXXXXX";
    /**
    * The AccessKeySecret that is created in the Alibaba Cloud console for identity authentication.
    */
    String secretKey = "XXXXXXXXXXXXXXXXX";
    IClientProfile profile= DefaultProfile.getProfile(regionId，accessKey，secretKey);
    IAcsClient iAcsClient= new DefaultAcsClient(profile);

			
```

**Parameters**

The following table lists the parameter settings required to start the API client.

|Name|Description|
|----|-----------|
|regionId|The region where the API gateway is located.|
|accessKey|The AccessKeyId that is obtained in the Alibaba Cloud console.|
|secretKey|The AccessKeySecret that is obtained in the Alibaba Cloud console.|

**Region list**

The following table lists the regions that are supported by the MQ for MQTT API.

|Region|Region ID|Remarks|
|------|---------|-------|
|Public cloud China \(Beijing\)|cn-beijing|Applicable to users who use the public cloud China \(Beijing\) region|
|Internet|mq-internet-access|Applicable to users who use public regions|
|Public cloud China \(Hangzhou\)|cn-hangzhou|Applicable to users who use the public cloud China \(Hangzhou\) region|
|Public cloud China \(Shanghai\)|cn-shanghai|Applicable to users who use the public cloud China \(Shanghai\) region|
|Public cloud China \(Shenzhen\)|cn-shenzhen|Applicable to users who use the public cloud China \(Shenzhen\) region|
|Public cloud Singapore|ap-southeast-1|Applicable to users who use the public cloud Singapore region|
|AntCloud China \(Hangzhou\)|cn-hangzhou-finance|Applicable to users who use the AntCloud China \(Hangzhou\) region|
|AntCloud China \(Shenzhen\)|cn-shenzhen-finance-1|Applicable to users who use the AntCloud China \(Shenzhen\) region|
|AntCloud China \(Shanghai\)|cn-shanghai-finance-1|Applicable to users who use the AntCloud China \(Shanghai\) region|

