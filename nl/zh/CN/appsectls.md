---

copyright:

  years: 1994, 2018

lastupdated: "2018-01-05"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:new_window: target="_blank"}

# 撤销对 TLS 1.0 和 1.1 的支持
{: #tlssupportwithdraw}

IBM 于 2018 年 3 月 1 日撤销了对许多云产品和服务上 TLS 1.0 和 TLS 1.1 的支持。对于要撤销对 TLS 1.0 和 1.1 的支持的任何 {{site.data.keyword.Bluemix}} 产品或服务，将继续支持 TLS 1.2。
{:shortdesc}

## 为什么 TLS 版本支持会更改？
{: #why}

通过更改 TLS 版本支持，可提供安全且符合关于安全和数据隐私的业界最佳实践的云环境。

## 什么是 TLS？
{: #what}

[TLS 协议 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/Transport_Layer_Security){: new_window} 用于对整个网络中的通信进行加密，以确保传输的数据始终保持私有。TLS 已发布了以下版本：1.0、1.1 和 1.2。所有 HTTPS 连接都使用 TLS。HTTPS 是用于确保与 {{site.data.keyword.Bluemix_notm}} 产品和服务的连接可信且安全的主要方法。一些 {{site.data.keyword.Bluemix_notm}} 产品和服务允许使用 WebSocket Secure (WSS) 协议（此协议也使用 TLS）来建立安全的连接。撤销对 TLS 1.0 和 1.1 的支持涵盖 HTTPS 和 WSS 连接。

## 需要采取什么行动来确保不会受到影响？
{: #impact}

与 {{site.data.keyword.Bluemix_notm}} 产品或服务建立的绝大部分连接已经在使用 TLS 1.2。如果连接不需要 TLS 1.0 或 1.1，那么不会受到影响。

如果要使用将撤销对 TLS 1.0 或 1.1 的支持的任何产品或服务，您必须确认连接不需要 TLS 1.0 或 1.1。

### {{site.data.keyword.Bluemix_notm}} 上的 Cloud Foundry

对于 Cloud Foundry 应用程序，您必须确认从 {{site.data.keyword.Bluemix_notm}} 外部连接到应用程序不会受到影响。同时确认从应用程序连接到 {{site.data.keyword.Bluemix_notm}} 上的其他 Cloud Foundry 应用程序也不受影响。

与 Cloud Foundry 的所有使用 TLS 的连接都可能会受到影响，包括从 Web 浏览器建立的任何连接。所有新式浏览器都支持 TLS 1.2，包括作为 {{site.data.keyword.Bluemix_notm}} [先决条件](/docs/overview/prereqs.html#browsers)的浏览器。
{: tip}

#### 连接到 Cloud Foundry 应用程序

`*.mybluemix.net` 域上的所有 Cloud Foundry 应用程序端点都可以通过仅支持 TLS 1.2 的备用端点进行访问。

要使用备用端点，请在应用程序的子域后添加 `alt.`。例如，如果应用程序在 `https://myapplication.mybluemix.net` 上托管，请使用 `https://myapplication.alt.mybluemix.net`。或者，对于 `https://myapplication.eu-gb.mybluemix.net`，请使用 `https://myapplication.alt.eu-gb.mybluemix.net`。

如果能够成功连接到备用端点，说明您不会受到影响。

如果无法成功连接，您必须更改客户机、客户机库或客户机配置以启用 TLS 1.2。

#### 在 Cloud Foundry 应用程序之间进行连接

使用以下命令将 Cloud Foundry 应用程序配置为连接到其他应用程序时，自动重定向到 `*.mybluemix.net` 域上提供的备用端点：
```
cf set-env <application_name> BLUEMIX_TLS10_DISABLED true
```

将 `BLUEMIX_TLS10_DISABLED` 设置为 `true` 后，必须使用以下命令对应用程序重新编译打包，以使此更改生效：
```
cf restage <application_name>
```

进行这些更改后，从应用程序发起的任何出站请求都将重定向到仅支持 TLS 1.2 的备用端点。

要使用备用端点，客户机必须支持服务器名称指示 (SNI) TLS 扩展。否则，客户机可能会将返回的证书视为无效，并且您可能错误地假定自己会受 TLS 1.0 和 1.1 除去操作的影响。
{: tip}

### Watson 产品和服务

对于 Watson 产品和服务，请对您的连接进行以下替换：
  * 将 `gateway.watsonplatform.net` 替换为 `gateway-tls12.watsonplatform.net`
  * 将 `stream.wastonplatform.net` 替换为 `stream-tls12.watsonplatform.net`

这两个备用端点仅支持 TLS 1.2。如果能够成功连接到这两个备用端点，说明您不会受到影响。如果无法成功连接，您必须更改客户机、客户机库或客户机配置以启用 TLS 1.2。

达拉斯以外位置中的 Watson 产品和服务已经仅支持 TLS 1.2，因此没有为这些产品和服务提供备用端点。

`gatway-tls12.watsonplatform.net` 和 `stream-tls12.watsonplatform.net` 仅用于测试目的，在除去 TLS 1.0 和 1.1 之后将不可用。
{: tip}

### 其他产品或服务

对于没有仅支持 TLS 1.2 的备用端点可用的产品或服务，请参阅客户机的任何可用文档。有关如何确定支持的 TLS 版本以及正在使用的版本的信息，请参阅客户机库。 

## 哪些产品和服务将撤销对 TLS 1.0 和 1.1 的支持？
{: #prodsandservs}

以下产品或服务将撤销对 TLS 1.0 和 1.1 的支持。

某些产品或服务（例如，{{site.data.keyword.Bluemix_notm}} 上的 Cloud Foundry 和 {{site.data.keyword.Bluemix_notm}} 目录中的服务）可能会在多个位置中提供。将在当前受支持的所有位置中除去 TLS 1.0 和 1.1。

不包括 {{site.data.keyword.Bluemix_notm}} Private 或 {{site.data.keyword.Bluemix_notm}} Local System 部署，也不包括这些部署中托管的任何 {{site.data.keyword.Bluemix_notm}} 服务。如果部署仍支持 TLS 1.0 或 1.1，请与客户或支持代表一起确定何时除去这些支持对您来说比较合适。
{: note}

### 通过 {{site.data.keyword.Bluemix_notm}} 目录提供的产品或服务

#### 云平台

* {{site.data.keyword.Bluemix_notm}} 上的 Cloud Foundry
* {{site.data.keyword.Bluemix_notm}} 基础架构（`api.softlayer.com` 和 `api.service.softlayer.com`）

#### API

* API Connect

#### 应用程序服务

* Business Rules
* Message Hub
* Voice Agent with Watson\*
* Watson Content Knowledge Kits\*

#### 数据和分析

* Compose Enterprise
* Compose for Elasticsearch
* Compose for etcd
* Compose for JanusGraph
* Compose for MongoDB
* Compose for MySQL
* Compose for PostgreSQL
* Compose for RabbitMQ
* Compose for Redis
* Compose for RethinkDB
* Compose for ScyllaDB
* Data Catalog
* Data Connect
* Data Refinery
* Decision Optimization
* Graph
* Informix
* Lift
* Weather Company Data
* Managed MS-SQL Database Server\*

#### DevOps

* Auto-Scaling
* Alert Notification
* Availability Monitoring
* Continuous Delivery
* Continuous Release
* DevOps Insights
* Event Management
* Globalization Pipeline
* Automated Accessibility Tester\*
* Digital Content Checker\*
* Runbook Automation\*

#### 财务

* Historical Instrument Analytics\*
* Instrument Analytics\*
* Investment Portfolio\*
* Portfolio Optimization\*
* Predictive Market Scenarios\*
* Simulated Historical Instrument Analytics\*
* Simulated Instrument Analytics\*

#### 功能

* 功能

#### 集成

* App Connect
* Product Insights
* Secure Gateway
* API Harmony\*

#### Internet of Things

* IoT for Electronics

#### 移动

* App ID†
* Mobile Analytics
* Mobile Foundation
* Push Notifications
* App Launch\*

#### 安全性

* App ID†
* SSL 证书†

#### Watson

* Conversation
* Discovery
* Language Translator
* Natural Language Classifier
* Natural Language Understanding
* Personality Insights
* Speech to Text
* Text to Speech
* Tone Analyzer
* Visual Recognition
* Knowledge Studio\*
* Document Conversion‡
* Retrieve & Rank‡
* Tradeoff Analytics‡

\* 在 {{site.data.keyword.Bluemix_notm}}“目录”中的试验性服务下可用。  
† TLS 1.0 先前已除去，将仅除去 TLS 1.1。  
‡ 不推荐使用，仅可供现有客户使用。

### 通过 IBM Marketplace 提供的产品或服务

* Forms Experience Builder on Cloud
* IoT for Insurance
* Watson Customer Insight for Insurance
* Watson Marketing Insights
* Watson Knowledge Studio
* Watson Virtual Agent
* Weather Company Energy Trader

### 其他产品或服务
{: #prodservices}

* Teacher Advisor with Watson

## 如果未列出我的产品或服务该怎么办？
{: #tlsprodnotlisted}

您的产品或服务可能已经仅支持 TLS 1.2，或者可能现在不会除去 TLS 1.0 和 1.1。您可以使用各种客户机和联机工具来检查产品或服务的端点是否支持 TLS 1.0 和 1.1。

## 撤销支持后，有办法可以继续使用 TLS 1.0 或 1.1 吗？
{: #tlskeepusing}

某些产品和服务将启用备用端点，在从主端点除去 TLS 1.0 和 1.1 之后，这些备用端点将继续支持 TLS 1.0 和 1.1。

### {{site.data.keyword.Bluemix_notm}} 基础架构

从 `api.softlayer.com` 和 `api.service.softlayer.com` 中除去对 TLS 1.0 和 1.1 的支持后，将发布备用端点并有 30 天的可用期。

### Watson 产品和服务
{: #watsonprodservices}

要在撤销支持后，继续使用 TLS 1.0 或 1.1 来连接到 Watson 产品和服务，可以执行以下其中一个替换：
  * 将 `gateway.watsonplatform.net` 替换为 `gateway-tls10.wastonplatform.net`
  * 将 `stream.watsonplatform.net` 替换为 `stream-tls10.watsonplatform.net`

从 `gateway.patsonplatform.net` 和 `stream.watsonplatform.net` 中除去 TLS 1.0 和 1.1 后，您可以继续使用 `gateway-tls10.watsonplatform.net` 和 `stream-tls10.watsonplatform.net` 来支持 TLS 的这些版本。

## 取得联系
{: #tlssupport}

如果您有任何疑问、顾虑或问题，请[联系支持人员 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/support){: new_window}
