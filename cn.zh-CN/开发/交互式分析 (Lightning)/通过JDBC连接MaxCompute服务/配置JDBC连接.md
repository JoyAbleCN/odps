---
keyword: [JDBC连接, JDBC URL]
---

# 配置JDBC连接

您需要获得MaxCompute项目的JDBC URL，才能将SQL客户端工具连接到该项目。

Maxcompute Lightning查询引擎基于PostgreSQL 8.2，仅支持对已有MaxCompute表进行SELECT查询，更多详情请参见[查询语法](https://www.postgresql.org/docs/8.2/static/queries.html)及[函数](https://www.postgresql.org/docs/8.2/static/functions.html)。

JDBC URL命名方式如下。

```
jdbc:postgresql://endpoint:port/database
```

连接参数说明如下。

|参数|取值|说明|
|:-|:-|:-|
|endpoint|所在区域不同网络环境下的Lightning访问域名。|详情请参见[MaxCompute Lightning访问域名Endpoint](/cn.zh-CN/开发/交互式分析 (Lightning)/访问域名Endpoint.md)，例如通过外网访问上海Region的服务使用`lightning.cn-shanghai.maxcompute.aliyun.com`。|
|port|443。|无。|
|database|填写MaxCompute的项目名称。|无。|
|user|访问用户的AccessKey ID。|无。|
|password|访问用户的AccessKey Secret。|无。|
|ssl|true。|MaxCompute Lightning服务端默认开启SSL服务，客户端需要使用SSL进行连接。|
|prepareThreshold|0。|可选。需要使用JDBC PrepareStatement功能时，建议设置`prepareThreshold=0`。|
|preferQueryMode|simple。|可选。当PreparedStatement中使用分区过滤条件时，建议使用此参数。|

例如，`jdbc:postgresql://lightning.cn-shanghai.maxcompute.aliyun.com:443/myproject`。

之后，您还需要配置user、password、SSL连接参数后才能连接成功。

您也可以将参数添加到JDBC URL中，直接连接到MaxCompute项目，举例如下。

```
jdbc:postgresql://lightning.cn-shanghai.maxcompute.aliyun.com:443/myproject?ssl=true& prepareThreshold=0&user=xxx&password=yyy
```

参数说明：

-   lightning.cn-shanghai.maxcompute.aliyun.com：华东2区域的Endpoint。
-   myproject：需要访问的MaxCompute项目名称。
-   ssl=true：指定通过SSL方式连接。
-   xxx：访问用户的AccessKey ID。
-   yyy：访问用户的AccessKey Secret。

