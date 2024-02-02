---
title: 访问统一配置文件
description: 使用API访问统一配置文件。
exl-id: c9d2fa2d-9ffe-4e66-996f-ad930bee22c6
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '675'
ht-degree: 0%

---

# 使用配置文件API访问统一配置文件

Adobe [!DNL Experience Platform] 可以实时访问客户档案； [[!DNL Experience Platform] 实时客户资料API](https://adobe.ly/2TtDHWr) 是专门为与它互动而设计的。 查看此 [教程](https://docs.adobe.com/content/help/en/experience-platform/profile/api/getting-started.html) 了解如何使用配置文件API访问实时客户配置文件数据。

本文将在很大程度上参考上面链接的教程。

此 [Postman收藏集](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) 整篇文章都使用关联的按号码调用了。 有关安装和使用Postman集合的更多详细信息，请访问Github [自述文件](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) 页面。 还有以下示例数据集 [忠诚度](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) 和 [个人资料](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) 数据。

对于此部分，请使用Postman文件夹5：配置文件查找，5a：实时查找配置文件数据或5b：实时查找事件数据。

## 使用 API

以下部分可帮助您对Experience Platform进行身份验证。 了解API路径、标头信息等。

### 身份验证对象 [!DNL Platform]

请参阅 [此](https://docs.adobe.com/content/help/en/experience-platform/tutorials/authentication.html) 在执行以下任何调用之前编写身份验证教程。

### API路径

实时客户个人资料API所需的平台网关URL是： `https://platform.adobe.io/`

API的基本路径为： `/data/core/ups/access/entities`

完整的路径示例为： `https://platform.adobe.io/data/core/ups/access/entities`

### 标题信息

标头必须包括：

* Authorization
* x-gw-ims-org-id — 通过console.adobe.io获取
* x-api-key — 通过console.adobe.io获取
* x-sandbox-name — 从Adobe集成管理器获取
* Content-Type： application/json

有关标头的更多信息，请参阅 [教程](https://adobe.ly/2PTHuKv).

## 使用身份访问实时客户个人资料

利用配置文件API，可通过GET请求使用身份访问配置文件。 以下部分将遵循此说明 [指南](https://docs.adobe.com/content/help/en/experience-platform/profile/api/entities.html).

### 使用身份访问配置文件数据

该API允许使用身份访问用户档案信息。 这是通过使用实体ID作为参数和实体ID命名空间之一向/access/entities发出GET请求来完成的。 注意：请记住，任何返回50条记录的请求将只传递422 HTTP状态和显示“相关标识过多”的消息，并且搜索将需要使用更多参数来缩小。

请求：

以下请求使用标识检索客户的电子邮件和名称：

```
curl -X GET \
  'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.profile&entityId=janedoe@example.com&entityIdNS=email&fields=identities,person.name,workEmail' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

响应：

```
{
    "BVrqzwVv7o2p3naHvnsWpqZXv3KJgA": {
        "entityId": "BVrqzwVv7o2p3naHvnsWpqZXv3KJgA",
        "sources": [
            "1000000000"
        ],
        "entity": {
            "identities": [
                {
                    "id": "89149270342662559642753730269986316601",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "janedoe@example.com",
                    "namespace": {
                        "code": "email"
                    }
                },
                {
                    "id": "janesmith@example.com",
                    "namespace": {
                        "code": "email"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316604",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "58832431024964181144308914570411162539",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316602",
                    "namespace": {
                        "code": "ecid"
                    },
                    "primary": true
                }
            ],
            "person": {
                "name": {
                    "firstName": "Jane",
                    "middleName": "F",
                    "lastName": "Doe"
                }
            },
            "workEmail": {
                "primary": true,
                "address": "janedoe@example.com",
                "label": "Jane Doe",
                "type": "work",
                "status": "active"
            }
        },
        "lastModifiedAt": "2018-08-28T20:57:24Z"
    }
}
```

### 按身份列表访问配置文件

此API通过使用对/access/entities端点的POST请求并在有效负载中提供身份，使用身份列表提供对配置文件的访问。 这些身份包含ID值(entityId)和身份命名空间(entityIdNS)。

请求：以下请求按标识列表检索多个客户的名称和电子邮件地址：

```
curl -X POST \
  https://platform.adobe.io/data/core/ups/access/entities \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "schema":{
        "name":"_xdm.context.profile"
    },
    "fields":["identities","person.name","workEmail"],
    "identities":[
        {
            "entityId":"89149270342662559642753730269986316601",
            "entityIdNS":{
                "code":"ECID"
            }
        },
        {
            "entityId":"89149270342662559642753730269986316900",
            "entityIdNS":{
                "code":"ECID"
            }
        },
        {
            "entityId":"89149270342662559642753730269986316602",
            "entityIdNS":{
                "code":"ECID"
            }
        }
    ]
}'
```

响应：成功的响应将返回请求正文中指定的实体的请求字段。

```
{
    "A29cgveD5y64ezlhxjUXNzcm": {
        "entityId": "A29cgveD5y64ezlhxjUXNzcm",
        "sources": [
            "1000000000"
        ],
        "entity": {
            "identities": [
                {
                    "id": "89149270342662559642753730269986316601",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "janedoe@example.com",
                    "namespace": {
                        "code": "email"
                    }
                },
                {
                    "id": "05DD23564EC4607F0A490D44",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316603",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "janesmith@example.com",
                    "namespace": {
                        "code": "email"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316604",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316700",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316701",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "58832431024964181144308914570411162539",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316602",
                    "namespace": {
                        "code": "ecid"
                    },
                    "primary": true
                }
            ],
            "person": {
                "name": {
                    "firstName": "Jane",
                    "middleName": "F",
                    "lastName": "Doe"
                }
            },
            "workEmail": {
                "primary": true,
                "address": "janedoe@example.com",
                "label": "Jane Doe",
                "type": "work",
                "status": "active"
            }
        },
        "lastModifiedAt": "2018-08-28T20:57:24Z"
    },
    "A29cgveD5y64e2RixjUXNzcm": {
        "entityId": "A29cgveD5y64e2RixjUXNzcm",
        "sources": [
            ""
        ],
        "entity": {},
        "lastModifiedAt": "1970-01-01T00:00:00Z"
    },
    "A29cgveD5y64ezphxjUXNzcm": {
        "entityId": "A29cgveD5y64ezphxjUXNzcm",
        "sources": [
            "1000000000"
        ],
        "entity": {
            "identities": [
                {
                    "id": "89149270342662559642753730269986316602",
                    "namespace": {
                        "code": "ecid"
                    },
                    "primary": true
                },
                {
                    "id": "janedoe@example.com",
                    "namespace": {
                        "code": "email"
                    }
                }
            ],
            "person": {
                "name": {
                    "firstName": "Jane",
                    "middleName": "F",
                    "lastName": "Doe"
                }
            },
            "workEmail": {
                "primary": true,
                "address": "janedoe@example.com",
                "label": "Jane Doe",
                "type": "work",
                "status": "active"
            }
        },
        "lastModifiedAt": "2018-08-27T23:25:52Z"
    }
}
```

## 时间序列事件

合作伙伴可以通过向/access/entities端点发出GET请求，通过关联的配置文件实体的身份访问时序事件。

### 按身份访问用户档案的时间系列事件

通过向/access/entities端点发出GET请求，可使用关联配置文件实体的身份访问时间序列事件。 此身份包含ID值(entityId)和身份命名空间(entityIdNS)。

请求：以下请求按ID查找配置文件实体，并检索endUserIDs、web和channel属性的值 **全部** 与实体关联的时间序列事件。

```
curl -X GET \
  'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

响应：

成功的响应将返回在请求参数中指定的时间序列事件和相关字段的分页列表。

```
{
    "_page": {
        "orderby": "timestamp",
        "start": "c8d11988-6b56-4571-a123-b6ce74236036",
        "count": 1,
        "next": "c8d11988-6b56-4571-a123-b6ce74236037"
    },
    "children": [
        {
            "relatedEntityId": "A29cgveD5y64e2RixjUXNzcm",
            "entityId": "c8d11988-6b56-4571-a123-b6ce74236036",
            "timestamp": 1531260476000,
            "entity": {
                "endUserIDs": {
                    "_experience": {
                        "ecid": {
                            "id": "89149270342662559642753730269986316900",
                            "namespace": {
                                "code": "ecid"
                            }
                        }
                    }
                },
                "channel": {
                    "_type": "web"
                },
                "web": {
                    "webPageDetails": {
                        "name": "Fernie Snow",
                        "pageViews": {
                            "value": 1
                        }
                    }
                }
            },
            "lastModifiedAt": "2018-08-21T06:49:02Z"
        }
    ],
    "_links": {
        "next": {
            "href": "/entities?start=c8d11988-6b56-4571-a123-b6ce74236037&orderby=timestamp&schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1"
        }
    }
}
```

### 用户档案的时间系列事件分页

检索时序事件时，结果将分页。 如果有后续的结果页，则响应的_page.next参数将包含一个ID。 此外，响应的_links.next.href参数提供了用于检索后续页面的请求URI。

请求：

以下请求通过使用_links.next.href URI作为请求路径来检索下一页结果。

```
curl -X GET \

  'https://platform.adobe.io/data/core/ups/access/entities?start=c8d11988-6b56-4571-a123-b6ce74236037&orderby=timestamp&schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

响应：

成功的响应将返回结果的下一页。 此示例演示了没有后续结果页的响应，如_page.next和_links.next.href的空字符串值所示。

```
{
    "_page": {
        "orderby": "timestamp",
        "start": "c8d11988-6b56-4571-a123-b6ce74236037",
        "count": 1,
        "next": ""
    },
    "children": [
        {
            "relatedEntityId": "A29cgveD5y64e2RixjUXNzcm",
            "entityId": "c8d11988-6b56-4571-a123-b6ce74236037",
            "timestamp": 1531260477000,
            "entity": {
                "endUserIDs": {
                    "_experience": {
                        "ecid": {
                            "id": "89149270342662559642753730269986316900",
                            "namespace": {
                                "code": "ecid"
                            }
                        }
                    }
                },
                "channel": {
                    "_type": "web"
                },
                "web": {
                    "webPageDetails": {
                        "name": "Fernie Snow",
                        "pageViews": {
                            "value": 1
                        }
                    }
                }
            },
            "lastModifiedAt": "2018-08-21T06:50:01Z"
        }
    ],
    "_links": {
        "next": {
            "href": ""
        }
    }
}
```

## 参考文章

* [实时客户资料API](https://adobe.ly/2TtDHWr)
* [使用配置文件API教程访问实时客户配置文件数据](https://docs.adobe.com/content/help/en/experience-platform/profile/api/getting-started.html)
* [[!DNL Experience Platform] Authentication指南](https://docs.adobe.com/content/help/en/experience-platform/tutorials/authentication.html)
