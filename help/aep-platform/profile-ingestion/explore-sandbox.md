---
title: 访问和探索AEP沙盒
description: 了解如何访问和探索Experience Platform沙盒。
exl-id: 62c21615-4b03-4900-a1ad-8f809c836491
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '737'
ht-degree: 0%

---

# 访问和探索AEP沙盒

本文涵盖以下内容：

* 现有AdobeExchange合作伙伴沙盒组织与共享AEP沙盒之间的区别。
* 正在请求访问AEP共享沙盒。
* 收到加入AEP共享沙盒的电子邮件邀请。
* 邀请新用户加入 [!DNL Admin Console].
* 浏览AEP UI。

有关AEP中沙盒技术的常规概述，请参阅此 [文章](https://docs.adobe.com/content/help/zh-Hans/experience-platform/sandbox/home.html).

## 共享的AEP沙盒

Exchange合作伙伴有权访问各种Adobe [!DNL Experience Cloud] products(非AEP产品，如 [!DNL Analytics]， [!DNL Target]、Platform标签等)通过其自己的Adobe [!DNL Experience Cloud] 组织（非共享）。 合作伙伴拥有对其自身组织的系统管理员访问权限以管理用户和其他权限。 Adobe [!DNL Experience Platform] (AEP)的处理方式与其他Adobe沙盒不同。 主要区别如下：

* 对AEP的访问不通过合作伙伴的主要Adobe [!DNL Experience Cloud] 沙盒组织。
* 可通过共享AdobeExchange组织访问AEP。
* 许多其他AdobeExchange合作伙伴公司正在使用同一组织访问AEP
   * 通过AEP沙盒功能，其他合作伙伴将无法查看或修改此共享组织内的数据和活动；每个合作伙伴将可以访问共享组织内的不同沙盒。
* 此共享组织内的管理权限非常有限。
* 授予合作伙伴访问AEP沙盒的权限后，他们将在UI的右上角的Org Switcher中看到两个组织，同时在Admin Console或主Experience Cloud主页中看到这两个组织。 但是，在登录到AEP时，应该只显示共享的组织。

## 请求访问共享AEP沙盒

提交 [支持请求](https://adobeexchangeec.zendesk.com/hc/en-us/requests/new) ，并提供以下信息：

* Email Address
* 主题： AEP沙盒请求
* 产品：常规配置/沙盒
* 票证类型：项目支持 — Exchange项目/设置请求问题
* 描述：简要描述需要使用AEP沙盒的集成用例
* 另请确保提供应添加到AEP沙盒的所有用户名和电子邮件。 在提出请求后可以添加其他用户，但需要通过Adobe通过其他票证添加用户（请参阅下文）。

## 接收电子邮件邀请

请求AEP沙盒的主要联系人将收到一封自动电子邮件，邀请他们“开始”使用Adobe [!DNL Experience Platform]. 主要联系人还将具有一些管理权限，这些权限将在下一节中介绍。

不要选择电子邮件中的“开始”按钮，而是直接导航到 `https://platform.adobe.com.` 使用与邀请中的电子邮件地址关联的Adobe ID登录，或者如果未与Adobe ID关联，则创建一个。

## 邀请其他用户

提交 [支持请求](https://adobeexchangeec.zendesk.com/hc/en-us/requests/new) ，并提供以下信息：

* 请求者电子邮件地址
* 主题： AEP沙盒 — 添加管理员/用户
* 产品：常规配置/沙盒
* 票证类型：项目支持 — Exchange项目/设置请求问题
* 描述：要添加的用户的列表（名称和电子邮件）

## 浏览AEP UI

观看AEP UI [简介视频](https://docs.adobe.com/content/help/en/platform-learn/tutorials/intro-to-platform/interface-tour.html)

AEP UI中有12个主要区域，可通过左侧面板导航。 但是，对于此类集成，最重要的部分是“架构”、“数据集”和“配置文件”。

* 主页 — 登陆屏幕

   * 建议一些入门活动
   * 提供一些指向学习内容的链接
   * 提供某些主要AEP对象的仪表板视图，如架构、数据集和配置文件

* 工作流 — 启动到常用工作流中，将数据引入AEP
* 连接/源 — 管理进入AEP的数据源
* 连接/目标 — 管理连接以将数据发送到外部系统
* 配置文件 — 查看和管理单个客户配置文件
* 区段 — 浏览、创建和修改客户区段
* 身份 — 浏览、创建和修改身份命名空间；这些是用于唯一标识客户的主ID类型
* 模型（数据科学） — 参与数据科学活动，包括使用嵌入式Jupyter Notebooks环境
* 服务（数据科学） — 将数据科学方法作为服务发布
* 架构 — 浏览、创建和修改架构；这些是用于保持数据有序性的详细数据定义
* 数据集 — 浏览、创建和管理数据集；数据集由架构定义，并且数据驻留在AEP中
* 查询 — 浏览、创建、修改和使用查询存储库，以从数据集中的数据获得见解
* 监控 — 查看所有数据在AEP中的进出状态，包括批处理数据和流式数据
