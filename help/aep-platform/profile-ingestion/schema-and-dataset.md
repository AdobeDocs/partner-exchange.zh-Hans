---
title: 创建AEP模式和数据集
description: 在Experience Platform中创建架构和数据集。
exl-id: a2773551-20a3-4a5b-ab53-60fa67e38ec0
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '487'
ht-degree: 10%

---

# 创建架构和数据集

此 [Postman收藏集](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) 整篇文章都使用关联的按号码调用了。 有关安装和使用Postman集合的更多详细信息，请访问Github [自述文件](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) 页面。 还有以下示例数据集 [忠诚度](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) 和 [个人资料](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) 数据。

## 架构

架构是一组规则，用于表示和验证数据的结构和格式。在高层面上，架构提供了真实世界对象（如人）的抽象定义，并概括了该对象的每个实例中应包含哪些数据（如名字、姓氏、生日等）。 可以在UI中或使用以下方式创建架构 [!DNL Experience Platform] API。

请参阅 [本文档](https://www.adobe.io/apis/experienceplatform/home/xdm/xdmservices.html#!api-specification/markdown/narrative/technical_overview/schema_registry/schema_composition/schema_composition.md) 以了解更多详细信息。

### 创建架构

合作伙伴可以使用UI构建架构，方法是 [教程](https://docs.adobe.com/content/help/zh-Hans/experience-platform/xdm/tutorials/create-schema-ui.html). 此示例使用忠诚度计划用户档案架构。 虽然配置文件架构是一个示例，但可以使用基于事件的架构进行类似的过程。

要使用API，合作伙伴必须与现有Adobe I/O集成 [!DNL Experience Platform] 权限已启用。 请参阅本指南，以了解 [创建I/O集成](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md).

然后访问 [此链接](https://docs.adobe.com/content/help/en/experience-platform/xdm/tutorials/create-schema-api.html) 了解如何使用API构建架构。

要通过Postman创建架构，请使用文件夹1：创建架构、1a：为PROFILE数据创建架构或1b：为EVENT数据创建架构中包含的调用。

## 数据集

所有引入Adobe的数据 [!DNL Experience Platform] 包含在数据集中。 数据集是用于数据集合的存储和管理结构，通常是表格，其中包含架构（列）和字段（行）。数据集还包含描述其存储的数据的各个方面的元数据。

目录服务是数据位置和历程的记录系统。 [!DNL Experience Platform]，用于创建和管理数据集。 目录跟踪每个数据集的元数据，其中包括对数据集符合的体验数据模型(XDM)架构的引用（下一节中有说明）以及摄取到该数据集的记录数。

开始 [此处](https://docs.adobe.com/content/help/en/experience-platform/catalog/datasets/overview.html) 以了解详细的数据集概述。

### 创建数据集

![创建数据集动画Gif](images/creating_a_dataset.gif)

<!-- 
We don't yet support hover text in images (and we render it poorly when included). I removed "Creating a Dataset" from the above image link. We can add it back when we support it (Summer 2020?) -Bob
-->

通过UI创建数据集：

1. 单击 **[!UICONTROL 创建数据集]**.

1. 单击 **[!UICONTROL 从架构创建]**.

1. 单击&#x200B;**[!UICONTROL 完成]**。

开始 [此处](https://docs.adobe.com/content/help/en/experience-platform/catalog/datasets/user-guide.html) 用于数据集用户指南。

[使用API创建数据集](https://docs.adobe.com/content/help/en/experience-platform/catalog/datasets/create.html).

要通过Postman创建数据集，请使用文件夹2：创建数据集，2a：为配置文件数据创建数据集，或2b：为EVENT数据创建数据集。

## 适用于合作伙伴的架构和数据集最佳实践

* 与为客户现有的配置文件架构和体验架构创建混合模式相比，合作伙伴数据应使用单独的配置文件架构。
* 合作伙伴应尽可能使用Adobe类和混合语言。
* 合作伙伴应使用单独的数据集上传其数据，而不是尝试将其数据合并到现有的数据集中。
* 合作伙伴目前无法将其架构上传到全局注册表。
