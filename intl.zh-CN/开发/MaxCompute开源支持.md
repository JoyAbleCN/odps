---
keyword: [开源功能, SDK, MaxCompute RODPS, MaxCompute JDBC, Mars, Data collector]
---

# MaxCompute开源支持

MaxCompute是阿里巴巴自研的一站式的快速、完全托管的TB/PB级数据仓库解决方案。本文将为您介绍MaxCompute相关的开源功能。

## SDK

MaxCompute提供Java SDK和Python SDK接口实现创建、查看、删除MaxCompute表等操作。通过SDK，您可以通过编辑代码灵活地操作MaxCompute。MaxCompute提供的SDK如下：

-   Java SDK

    使用说明请参见[SDK 参考 \> Java SDK](/intl.zh-CN/SDK参考/Java SDK/Java SDK介绍.md)。

    较为常用的MaxCompute核心接口详情请参见[Java SDK](https://github.com/aliyun/aliyun-odps-java-sdk)。更多信息可参见GitHub [ODPS SDK for Java Developers](https://github.com/aliyun/aliyun-odps-java-sdk)。

    **服务支持方式**：您可通过访问官方文档和在线[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex)的方式获取支持。

-   Python SDK

    PyODPS是MaxCompute的Python版本的SDK，提供对MaxCompute对象的基本操作和DataFrame框架，让您可以轻松地在MaxCompute上进行数据分析。更多详情请参见GitHub项目[aliyun-odps-python-sdk](https://github.com/aliyun/aliyun-odps-python-sdk)和包括所有接口、类的细节等详细内容的[PyODPS文档]()。

    -   了解更多关于PyODPS的开发内容，请参见[PyODPS开发指南]()。在您开始使用PyODPS之前请先安装PyODPS，详细内容请参见[PyODPS安装指南](/intl.zh-CN/开发/PyODPS/安装指南及使用限制.md)。
    -   关于PyODPS的社区信息请参见[PyODPS云栖社区专辑](https://yq.aliyun.com/album/19?spm=a2c4g.11186623.2.25.5ce96074vn9tRs)。
    -   欢迎各位开发者参与到PyODPS的生态开发中，详情请参见[PyODPS DataFrame概述]()。
    -   欢迎反馈问题和需求，加快PyODPS生态成长，更多详情请参见GitHub [ODPS Python SDK and data analysis framework](https://github.com/aliyun/aliyun-odps-python-sdk)。
    **服务支持方式**：您可通过访问官方文档和在线[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex)的方式获取支持。


## MaxCompute RODPS

MaxCompute R语言插件：RODPS。具体使用说明请参见GitHub [ODPS Plugin for R](https://github.com/aliyun/aliyun-odps-r-plugin)。

**服务支持方式**：在GitHub[ODPS Plugin for R](https://github.com/aliyun/aliyun-odps-r-plugin)中留言或新建Issue。

## MaxCompute JDBC

odps-jdbc是MaxCompute官方提供的JDBC驱动，它向Java程序提供了一套执行SQL任务的接口。项目托管在GiHub [ODPS JDBC](https://github.com/aliyun/aliyun-odps-jdbc)。

**服务支持方式**：在GitHub [ODPS JDBC](https://github.com/aliyun/aliyun-odps-jdbc)中留言或新建Issue。

## Mars

Mars是一个基于张量的统一分布式计算框架。使用Mars进行科学计算，不仅使大规模科学计算任务的实现从MapReduce上的数千行代码降低到Mars上的数行代码，更在性能上有大幅提升。

Mars已经在Github上开源代码，您可以一起参与共建Mars。请参见GitHub开源代码[Mars](https://github.com/mars-project/mars) 。

关于Mars的更多说明信息请参见[Mars开发指南](https://mars-project.readthedocs.io/en/latest/)。您还可通过云栖文章了解Mars，请参见[Mars相关文档](https://yq.aliyun.com/search?q=Mars)。

**服务支持方式**：在Github [Mars](https://github.com/mars-project/mars)中留言或新建Issue。

## Data collector

MaxCompute数据集成工具插件，包括Flume、OGG、kettle、Sqoop、Hive 。

Data Collector是MaxCompute主要开源数据采集工具的集合，包括：

-   Flume插件
-   OGG插件
-   Sqoop
-   Kettle插件
-   Hive Data Transfer UDTF

    从这些数据采集工具的实现技术上来看，Flume和OGG插件是基于DataHub的SDK实现，而Sqoop、Kettle以及Hive Data Transfer UDTF是基于Tunnel的SDK实现。DataHub和Tunnel分别是MaxCompute系统的实时和批量数据通道。Flume插件和OGG插件面向于实时数据传输，而Sqoop、Kettle和Hive Data Transfer UDTF则适合用于离线批量的数据传输。


源代码请参见GitHub源码[Aliyun MaxCompute Data Collectors](https://github.com/aliyun/aliyun-maxcompute-data-collectors)。插件说明请参见[wiki](https://github.com/aliyun/aliyun-maxcompute-data-collectors/wiki)。

**服务支持方式**：在GitHub [Aliyun MaxCompute Data Collectors](https://github.com/aliyun/aliyun-maxcompute-data-collectors)中留言或新建Issue。

