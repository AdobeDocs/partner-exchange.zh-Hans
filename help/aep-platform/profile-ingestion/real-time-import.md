---
title: 实时导入
description: 了解如何实时将数据导入AEP。
exl-id: 0b6215a9-1160-49ae-8aa5-302b47357200
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# 将数据流式传输到AEP

Adobe [!DNL Experience Platform] 允许对配置文件和体验事件进行流式传输并近乎实时地提供。 所有通过流式传输发送到AEP的数据都会保留在数据湖中。 Adobe可以通过API或使用Launch将数据流式传输到现有数据集或全新的数据集。

本文将介绍以下内容：

* 流式传输到XDM个人资料
* 流式传输到XDM ExperienceEvent
* 使用AEP Launch扩展进行流式传输

此 [Postman收藏集](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) 整篇文章都使用关联的按号码调用了。 有关安装和使用Postman集合的更多详细信息，请访问Github [自述文件](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) 页面。 还有以下示例数据集 [忠诚度](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) 和 [个人资料](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) 数据。

## 先决条件

* [验证平台](https://docs.adobe.com/content/help/en/experience-platform/tutorials/authentication.html).
* 从上面链接的身份验证教程中收集所需标头的值。

## 创建流连接

要流式传输到AEP，您必须先创建流式连接。 流连接包含流数据源等属性，以及您是否发送的记录属于 [!DNL Experience Data Model] (XDM)架构。 创建流连接后，您会获得一个唯一的URL，用于将数据流式传输到AEP中。

开始 [此处](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/create-streaming-connection.html) 有关如何通过API创建流连接的说明，或者 [此处](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/create-streaming-connection-ui.html) 有关如何通过UI创建流连接的说明。

```json
curl -X POST https://platform.adobe.io/data/foundation/flowservice/connections \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
     "name": "Sample name",
     "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
     "description": "Sample description",
     "connectionSpec": {
         "id": "bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb",
         "version": "1.0"
     },
     "auth": {
         "specName": "Streaming Connection",
         "params": {
             "sourceId": "Sample connection",
             "dataType": "xdm",
             "name": "Sample connection"
         }
     }
 }
```

响应：

```json
 {
    "id": "77a05521-91d6-451c-a055-2191d6851c34",
    "etag": "\"a500e689-0000-0200-0000-5e31df730000\""
}
```

请务必保存上述响应中提供的ID，以便将来进行流式摄取调用(Postman收藏集会将此ID保存在CONNECTION_ID环境变量中)。

## 将配置文件数据流式传输到AEP

对于此部分，请使用Postman调用文件夹： 3：实时导入，3a：配置文件数据的实时导入。

详细的JSON请求及流式配置文件数据的响应已记录在案 [此处](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/streaming-record-data.html).

步骤：

1. 创建XDM个人资料架构
1. 为XDM Individual Profile（主键）设置主身份描述符
1. 为XDM个人资料记录创建数据集
1. 调用流摄取API以创建XDM个人资料记录
1. 检索新创建的配置文件

## 将体验事件流式传输到AEP

对于此部分，请使用Postman调用文件夹： 3：实时导入，3b：配置文件数据的实时导入。

详细的JSON请求及流式体验数据的响应已记录在案 [此处](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/streaming-time-series-data.html).

步骤：

1. 创建XDM ExperienceEvent架构
1. 为XDM ExperienceEvent设置主身份描述符（主键）
1. 为XDM ExperienceEvents创建数据集
1. 调用流摄取API以创建XDM ExperienceEvent
1. 检索新创建的事件

## 使用Experience Platform标记流式传输到AEP

Adobe [!DNL Experience Platform] Launch扩展提供了一种通过Launch流式传输到AEP的方法。 要了解更多信息，请参阅 [本指南](https://docs.adobe.com/content/help/zh-Hans/launch/using/extensions-ref/adobe-extension/aep-extension/overview.html).

## 参考文章

* [数据摄取API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#/acpdr/swagger-specs)
* [流式摄取概述](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/streaming_ingest/streaming_ingest_overview.md)
* [流摄取开发人员指南](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/streaming_ingest/getting_started_with_platform_streaming_ingestion.md)
* [使用AEP Launch扩展](https://docs.adobe.com/content/help/zh-Hans/launch/using/extensions-ref/adobe-extension/aep-extension/overview.html)
