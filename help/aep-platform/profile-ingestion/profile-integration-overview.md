---
title: '"[!DNL Platform] Profile Ingestion and Access集成指南概述”'
description: 了解与的集成 [!DNL Experience Platform] 配置文件摄取和访问。
exl-id: a593511c-dd4c-4437-af73-f44d795cacb8
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---

# 集成指南： [!DNL Experience Platform] 配置文件摄取和访问

合作伙伴应使用本集成指南来帮助他们构建Adobe的入口和出口功能 [!DNL Experience Platform] (AEP)。 有API可用于批量摄取、流式摄取和统一配置文件访问（出口）。

为了协助发展， [Postman收藏集](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) 已由Adobe交换团队创建。 在整个集成指南中都会引用此Postman收藏集。

有关安装和使用Postman集合的更多详细信息，请访问Github [自述文件](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) 页面。 还有以下示例数据集 [忠诚度](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) 和 [个人资料](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) 数据。

## 集成用例示例：交互式语音应答系统

对于集成商， [!DNL Experience Platform] API提供了平台的所有功能（在整个用户界面中提供），但现在还能够创建客户工作流和自动化数据流。 作为集成商，您可以定期检查数据集的状态、设置新的数据摄取过程，并将您自己的受众激活解决方案与统一用户档案集成。

以交互式语音响应(IVR)系统和呼叫中心管理软件为例。 供应商可以使用 [!DNL Experience Platform] 用于在体验数据湖中摄取客户呼叫中心活动的历史信息的API。 如果数据是在XDM中摄取 `ExperienceEvent` 架构（表示客户交互的架构），这些交互可以毫无摩擦地直接引入统一用户档案服务。 在本例中， `callerId` 用作客户的标识符。 Identity Service将负责身份解析并协助Unified Profile Service将最近与呼叫中心交互的任何数据点添加到客户个人资料中。

客户下次致电呼叫中心时，将首先由IVR接听。 要个性化留言并投放专为来电者定制的优惠，IVR系统需要了解有关来电者的更多信息。 要实现API与统一配置文件的集成，请参阅此处。 IVR后端可以联系统一配置文件服务进行点查找。 然后，查阅仅适用于呼叫中心交互的用户档案属性或完整客户档案，后者也具有适用于其他接触点上的交互的属性。 通过组合来自多个数据源的数据、使用身份解析和统一配置文件，呼叫中心和IVR提供商可以提供量身定制的客户体验，并受Adobe支持 [!DNL Experience Platform]“

## 一般资源

* AEP [产品文档](https://docs.adobe.com/content/help/en/experience-platform/landing/documentation/overview.html).
* AEP [可扩展性](https://www.adobe.com/insights/experience-platform-api-extensibility.html).

## 有问题或反馈？

请通过以下网站提交所有问题和反馈： [支持渠道](https://adobeexchangeec.zendesk.com/hc/en-us/requests/new)
