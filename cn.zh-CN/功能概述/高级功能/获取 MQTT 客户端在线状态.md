# 获取 MQTT 客户端在线状态 {#concept_50069_zh .concept}

您可通过调用同步查询接口或使用异步上下线通知的方式获取微消息队列 MQTT 客户端（下文简称为客户端）当前在线情况。

 微消息队列 MQTT 需结合后端存储消息队列产品，如消息队列 RocketMQ 等使用，来和部署在云端服务器上的应用（下文简称为业务应用）完成业务流程。

获取客户端在线状态这个功能主要的应用场景如下：

-   主业务流程中需要根据客户端是否在线来决定后续运行逻辑；
-   运维过程需要判断特定客户端当前是否在线；
-   业务应用需要在客户端上线或者下线时触发一些预定义的动作。

## 基本原理 {#section_1e4_ef7_7m2 .section}

 微消息队列 MQTT 服务器（下文简称为 MQTT 服务器）提供以下方式获取客户端在线状态：

-   调用同步查询接口

    该方式相对简单，即通过开放的接入点地址调用 HTTP/HTTPS 方式的 API 查询某个特定客户端的当前实时状态，适用于对单个客户端的状态判断。

-   异步上下线通知

    该方式使用消息通知，在客户端上线和下线事件触发时，MQTT 服务器会向后端存储消息队列推送一条上下线消息。业务应用一般部署在阿里云的服务器上，业务应用通过向后端存储消息队列订阅这条消息来获取所有客户端的上下线动作。

    该方式属于异步感知客户端的状态，且感知到的是上下线事件，而非在线状态，云端应用需要根据事件发生的时间序列分析出客户端的状态。


两种查询方式的区别如下：

-   同步查询接口是查询当前客户端的实时状态，理论上比异步通知的方式更精确，但只能查询单个客户端状态。
-   异步上下线通知因为采用消息解耦，状态判断更加复杂，且误判可能性更大，但该方法可以基于事件分析多个客户端的运行状态轨迹。

## 同步查询接口（公测期间） {#section_rt0_d03_tdn .section}

同步查询接口功能目前处于公测期间，仅限公网测试地域（Region）使用，后续会在其他地域开放服务。MQTT 服务器对外通过 HTTP/HTTPS 协议方式暴露查询接口。

-    **查询接口** 

    ``` {#codeblock_xg4_k77_yd7}
    https://<mqtt-xxx.mqtt.aliyuncs.com\>/route/clientId/get 
    ```

    其中：

    -    <mqtt-xxx.mqtt.aliyuncs.com\> 需替换为您所购买的实例的接入点域名。
    -   协议可选 HTTP 或者 HTTPS，调用方式仅限 GET 方法。
-    **请求参数** 

    发送请求的时候所需带上的参数如[表 1](#table_zpr_wtx_i4w) 所述。

    |参数名称|参数类型|说明|
    |----|----|--|
    |accessKey|String|当前请求使用的账号的 AccessKeyId。|
    |resource|String|需要查询的客户端 Client ID，只能查询当前账号拥有的 Client ID。|
    |timestamp|Long|用于阻止非法过期请求，需要设置为当前真实时间，即该参数即构建当前请求的 UNIX 毫秒时间戳。时间戳过期时间为当前真实时间前后 5 分钟。|
    |instanceId|String|当前使用的实例 ID。|
    |signature|String|请求参数的签名，将其他参数作为待签名字符串，使用账户 AccessKeySecret 计算得到，用于权限验证和防止请求篡改。具体计算方式请参见[应用服务器 Token 接口规范](../intl.zh-CN/权限验证/Token 鉴权模式/Token 应用服务器接口/应用服务器 Token 接口规范.md#)。|

-    **返回结果** 

    MQTT 服务器接收到查询请求后，会验证接口参数，对于合法的查询请求返回当前客户端的在线连接数。具体返回情况如[表 2](#table_d6x_mdp_6aa) 所述。

    |HTTP 状态码|说明|
    |--------|--|
    |400|非法请求。原因可能是接口 URL 不正确或参数缺失。|
    |403|权限验证失败。原因可能是签名计算错误，或者没有权限查询对应的 Client ID 状态。|
    |200|请求处理成功。|

    返回结果中，除 HTTP 状态码，HTTP Content 中也会包含对应的返回结果，返回结果为 JSON 格式，具体的字段映射如[表 3](#table_780_eho_lqr) 所述。

    |字段名称|类型|说明|
    |----|--|--|
    |code|Integer|请求处理结果状态码，用于判断当前请求是否成功和失败原因。|
    |success|Boolean|查询是否成功，取值说明如下：     -   “true” 代表查询成功。
    -   “false” 代表查询失败。
 |
    |online|Integer|当前客户端的在线状态以及维持的 TCP 连接数。取值说明如下：     -   “0” 代表客户端不在线。
    -   大于 “0” 代表客户端在线。应用方应注意该字段大于 “1” 可能存在重复连接情况。因服务端数据同步延迟，会有很小概率出现返回 “-1” 的情况，代表查询结果未知，建议应用重试，或者基于实际场景做保守判断。
 |
    |message|String|请求处理结果描述，用于判断异常原因。|

    其中错误码（Error Code）说明如[表 4](#table_8s9_5pu_c8j) 所述。

    |code|说明|
    |----|--|
    |0|代表请求处理成功。|
    |1|参数缺失。建议检查参数对是否完整。|
    |2|签名计算错误。建议检查签名计算过程。|
    |3|权限验证错误。建议检查当前账号是否创建过该设备所属的 Group ID。|
    |4|请求时间戳过期。建议检查 timestamp 字段是否为最近 5 分钟内。|


## 异步上下线通知 {#section_grc_4n0_lcy .section}

如上文[基本原理](#section_1e4_ef7_7m2)所述，如果使用异步上下线通知的方式，上下线事件会映射到后端存储消息队列中。

下文以消息队列 RocketMQ 作为后端存储消息队列的情况为例说明。

 **操作步骤** 

1.  创建上下线事件对应的 Topic。

    您需关注哪些 Group ID 分组的设备，就在微消息队列 MQTT 控制台创建对应的 Topic。创建 Topic 的步骤请参见[MQTT 快速入门](../intl.zh-CN/快速入门/MQTT 快速入门.md#)中的**创建 Topic 和 Group ID** 部分。

    例如您需要关注 Group ID 为 GID\_XXX 类型的所有客户端，那么这类客户端对应的 Client ID 和 Topic 分别是 GID\_XXX@@@YYYYY 和 GID\_XXX\_MQTT。

    其中：

    -    GID\_XXX 是在微消息队列 MQTT 控制台上创建的 Group ID。
    -    YYYYY 是设备 ID，与 Group ID 以“<GroupID\>@@@<DeviceID\>” 模式构成 Client ID。
    -    \_MQTT 是该类事件通知消息的 Topic 命名所必需的固定后缀。
    更多信息请参见[名词解释](../intl.zh-CN/产品简介/名词解释.md#)。

2.  业务应用订阅该类通知消息。

    使用[步骤 1](#li_p5g_4qs_qnj) 中创建的 Topic，即可收到关注的客户端的上下线事件。消息队列 RocketMQ 的接收程序请参见[订阅消息](https://help.aliyun.com/document_detail/29551.html?spm=a2c4g.11186623.2.17.b6d83bcdcXvtmD)。

    事件类型放在消息队列 RocketMQ 的 Tag 中，代表上线或下线。数据格式如下：

     `RocketMQ Tag：connect/disconnect/tcpclean` 

    其中：

    -    connect 事件代表客户端上线动作。
    -    disconnect 事件代表客户端主动断开连接。按照 MQTT 协议，客户端主动断开 TCP 连接之前应该发送 disconnect 报文，MQTT 服务器在收到 disconnect 报文后触发该类型消息。如果某些客户端 SDK 没有按照协议发送 disconnect 报文，MQTT 服务器相应无法收到该消息。
    -    tcpclean 事件代表实际的 TCP 连接断开。无论客户端是否显示发送过 disconnect 报文，只要当前 TCP 连接断开就会触发 tcpclean 事件。
    **说明：** 

     tcpclean 消息代表客户端网络层连接的真实断开。对应的，disconnect 消息仅仅代表客户端是主动发送了下线报文。受限于客户端的实现，有时候客户端异常退出会导致 disconnect 消息并没有正常发送。因此判断客户端下线请使用 tcpclean 事件。

    数据内容为 JSON 类型，相关的 Key 说明如下：

    -    clientId 代表具体设备；
    -    time 代表本次事件的时间；
    -    eventType 代表事件类型，供客户端区分事件类型；
    -    channelId 代表每个 TCP 连接的唯一标识；
    -    clientIp 代表客户端使用的公网出口 IP 地址。
    示例：

    ``` {#codeblock_dyb_ddu_lpf}
    clientId：GID_XXX@@@YYYYY
    time:1212121212
    eventType:connect/disconnect/tcpclean
    channelId:2b9b1281046046faafe5e0b458e4XXXX
    clientIp：192.168.X.X:133XX     
    ```

    判断客户端当前是否在线不能仅仅根据收到的最后一条消息的状态，而需要结合上下线消息的前后关联来判断。

    具体判断规则如下：

    -   同一个 clientId 的客户端，产生上下线事件的先后顺序以时间为准，基本原则为时间戳越大则越新。
    -   同一个 clientId 的客户端，可能存在多次闪断，因此，当收到下线消息时，一定要根据 channelId 字段判断是否是当前的 TCP 连接。简而言之，下线消息只能覆盖 channelId 相同的下线消息，如果下线消息的 channelId 不一样，尽管 time 较新，也不能覆盖。

