---
title: 将批量数据导入AEP
description: 了解如何将批处理文件导入Experience Platform
exl-id: 50576b67-b3ba-498e-86f6-7e1986b76985
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 0%

---

# 将批量数据导入AEP

AEP可以从平面文件（如parquet）中摄取包含配置文件数据的批处理文件，或者从符合 [!UICONTROL 体验数据模型] (XDM)注册表。

AEP可以使用批处理文件摄取数据。 接受以下格式：JSON、Parquet和CSV。

本文将介绍以下内容：

* 批量摄取先决条件
* 批量摄取最佳实践和限制
* 如何创建批次
* 如何完成批处理
* 如何检查批次的状态

此 [Postman收藏集](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) 整篇文章都使用关联的按号码调用了。 有关安装和使用Postman集合的更多详细信息，请访问Github [自述文件](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) 页面。 还有以下示例数据集 [忠诚度](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) 和 [个人资料](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) 数据。

对于本教程中的所有调用，请使用Postman调用文件夹：4：批量导入、4a：PROFILE数据的批量导入或4b：EVENT数据的批量导入。

## 批量摄取先决条件

* 定义架构并创建数据集。
* 数据必须采用JSON、Parquet或CSV格式。
* [验证平台](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md).
* 从上面链接的身份验证教程中收集所需标头的值。

## 批量摄取最佳实践和限制

* 最大批次大小：100 GB
* 每批次的最大文件数：1500
* 如果文件大于512 MB，则需要将其划分为较小的块。 欲知更多详情，请参阅 [开发人员指南](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md)
* 每行属性或字段的最大数量：10,000
* 每用户每分钟最大批次数：138

## 创建批次

在本教程中，我们将使用JSON作为格式。 更多格式示例可在 [开发人员指南](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md)
使用JSON作为输入格式创建批处理（请确保包含数据集ID，并且您的数据符合链接到数据集的XDM架构）：

```json
curl -X POST "https://platform.adobe.io/data/foundation/import/batches" \
  -H "Accept: application/json" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}"
  -d '{
          "datasetId": "{DATASET_ID}",
           "inputFormat": {
                "format": "json"
           }
      }'
```

响应：

```json
{
    "id": "{BATCH_ID}",
    "imsOrg": "{IMS_ORG}",
    "updated": 0,
    "status": "loading",
    "created": 0,
    "relatedObjects": [
        {
            "type": "dataSet",
            "id": "{DATASET_ID}"
        }
    ],
    "version": "1.0.0",
    "tags": {},
    "createdUser": "{USER_ID}",
    "updatedUser": "{USER_ID}"
}
```

## 上载文件

现在可以将文件上载到新创建的批次（使用上面响应中的batch_id）。

```json
curl -X PUT "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.json" \
  -H "content-type: application/octet-stream" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}" \
  --data-binary "@{FILE_PATH_AND_NAME}.json"
```

>[!NOTE]
>
>API仅支持单部分上传，这意味着每个文件/微批次都需要通过单独的调用进行上传。 确保内容类型为application/octet-stream。

响应：

```
200 OK
```

## 完成批次

上传所有文件后，此调用将指示批次已准备好进行提升：

```json
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}"
```

响应：

```
200 OK
```

## 检查批次的状态

可在UI中或通过API检查批次状态（请参阅下面的调用）。 要签入UI，请导航到数据集以查看状态。

可以找到各种批次摄取状态 [此处](https://adobe.ly/2TMMCmj).


```json
curl GET "https://platform.adobe.io/data/foundation/catalog/batch/{BATCH_ID}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "x-api-key: {API_KEY}"
```

响应：

```json
{
    "{BATCH_ID}": {
        "imsOrg": "{IMS_ORG}",
        "created": 1494349962314,
        "createdClient": "MCDPCatalogService",
        "createdUser": "{USER_ID}",
        "updatedUser": "{USER_ID}",
        "updated": 1494349963467,
        "externalId": "{EXTERNAL_ID}",
        "status": "success",
        "errors": [
            {
                "code": "err-1494349963436"
            }
        ],
        "version": "1.0.3",
        "availableDates": {
            "startDate": 1337,
            "endDate": 4000
        },
        "relatedObjects": [
            {
                "type": "batch",
                "id": "foo_batch"
            },
            {
                "type": "connection",
                "id": "foo_connection"
            },
            {
                "type": "connector",
                "id": "foo_connector"
            },
            {
                "type": "dataSet",
                "id": "foo_dataSet"
            },
            {
                "type": "dataSetView",
                "id": "foo_dataSetView"
            },
            {
                "type": "dataSetFile",
                "id": "foo_dataSetFile"
            },
            {
                "type": "expressionBlock",
                "id": "foo_expressionBlock"
            },
            {
                "type": "service",
                "id": "foo_service"
            },
            {
                "type": "serviceDefinition",
                "id": "foo_serviceDefinition"
            }
        ],
        "metrics": {
            "foo": 1337
        },
        "tags": {
            "foo_bar": [
                "stuff"
            ],
            "bar_foo": [
                "woo",
                "baz"
            ],
            "foo/bar/foo-bar": [
                "weehaw",
                "wee:haw"
            ]
        },
        "inputFormat": {
            "format": "parquet",
            "delimiter": ".",
            "quote": "`",
            "escape": "\\",
            "nullMarker": "",
            "header": "true",
            "charset": "UTF-8"
        }
    }
}
```

## 参考文章

* [数据摄取API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#/acpdr/swagger-specs)
* [批量摄取概述](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/ingest_architectural_overview.md)
* [批量摄取开发人员指南](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md)
* [批量摄取疑难解答指南](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_troubleshooting_guide.md)
* [数据摄取Postman收藏集](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Data%20Ingestion%20API.postman_collection.json)
* [身份验证教程](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md)
