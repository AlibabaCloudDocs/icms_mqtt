# 什么是微消息队列 MQTT？ {#concept_42419_zh .concept}

本文主要介绍微消息队列 MQTT 的系统架构、应用场景和产品优势。如果说传统的消息队列大多应用于微服务之间，那么适用于物联网的微消息队列 MQTT 则实现了端与云之间的消息传递，真正意义上的万物互联。

关于如何使用微消息队列 MQTT，请参见[MQTT 快速入门](../intl.zh-CN/快速入门/MQTT 快速入门.md#)。

## 系统架构 {#section_vjb_1lh_hhb .section}

 微消息队列 MQTT 是阿里云推出的一款面向移动互联网以及物联网领域的轻量级消息中间件，针对移动互联网以及物联网 IoT 场景的消息传输特点，支持了包括 MQTT、STOMP、GB-808、新能源国标等主流通信协议。同时，微消息队列 MQTT 在数据传输层支持原生 TCP 长连接、SSL 加密、Websocket 等传输形式，支持包括 C/C++、Java、iOS、Android 等主流开发语言和平台。[图 1](#fig_tny_xjh_hhb) 展示了微消息队列 MQTT 的系统技术栈。

![](images/42260_zh-CN.png "系统架构")

## 应用场景 {#section_ekb_f3h_hhb .section}

得益于微消息队列 MQTT 的多协议、多语言和多平台的支持能力，目前广泛应用于移动互联网以及物联网领域，覆盖移动直播、车联网、金融支付、智能餐饮、即时聊天等多种应用场景。

 [图 2](#fig_yvq_4kh_hhb) 展示了微消息队列 MQTT 的主要应用场景。

![](images/42264_zh-CN.png "应用场景")

## 产品优势 {#section_lt3_2ph_kui .section}

 微消息队列 MQTT 主要承担移动端连接接入、连接管理、数据转发等工作，相当于一个无限扩展能力的连接网关，后端数据持久化和消息存储可以搭配阿里云其他消息队列产品，例如传统服务端消息中间件（消息队列 RocketMQ、消息队列 Kafka 等）。微消息队列 MQTT 系统采用分布式理念进行设计，无单点瓶颈，各组件之间均可以无限水平扩展，保证容量可以随着您的在线使用量进行调整，并且对用户完全透明。

 [图 3](#fig_vnq_rkh_hhb) 展示了微消息队列 MQTT 的产品优势。

![](images/42265_zh-CN.png "产品优势")

相比其他移动端消息服务，微消息队列 MQTT 具有以下优势：

-   支持的都是标准协议，例如 MQTT、STOMP、GB-808，应用方无技术捆绑，使用绝大多数开源的 SDK 即可无缝迁移到云上。
-   作为一个海量移动终端长连接网关，后端和阿里云其他消息产品数据打通，应用可以无需搭建自己的网关即可实现端和云的双向通信。
-   支持设备级权限控制，并支持 SSL/TLS 加密通信，数据传输更安全可靠。

