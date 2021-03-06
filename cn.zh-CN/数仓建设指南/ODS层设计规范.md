# ODS层设计规范 {#concept_226666 .concept}

本文为您介绍ODS层设计规范。

## 数据同步及处理规范 {#section_my0_mfd_xq7 .section}

-   数据同步方式的选择

    以下是数据同步的相关规范，基本规范以需求形式落地到DataWorks的数据集成实现，规范落地情况需要依赖工具的推进节奏：

    -   一个系统的源系统表只允许同步一次到MaxCompute。
    -   数据集成只做离线全量数据同步，增量数据同步可使用数据传输服务DTS的方式实现。
-   数据加载与处理
    -   数据集成同步时全量数据会直接进入全量表的当日分区。
    -   DTS同步数据主要有两步：
        -   全量初始化。对于同步的每个表，全量初始化的数据都会独立存储在MaxCompute中的全量基线表中，这个表名的默认格式为：源表名\_base。
        -   增量数据同步。增量数据数据实时同步到 MaxCompute中。并存储在增量日志表中，每个同步表对应一个增量日志表。在增量数据同步时，会使用合并多条记录到一个文件的方式写入到 MaxCompute中。增量日志表在MaxCompute中存储的表名的默认格式为：源表名\_log。
    -   DTS全量数据合并。根据同步到MaxCompute中的全量基线表和增量日志数据得到某个时刻表的全量数据，DTS 提供通过MaxCompute SQL实现全量数据合并的能力。通过DataWorks可以配置一个全量数据merge节点，当全量数据merge完成后，可自动调度起后续的计算分析节点。同时可以配置调度周期，进行周期性数据的离线分析。
    -   所有ODS层的表都以统计日期及时间分区表方式存储，数据成本由存储管理和策略部分控制和管理。
    -   数据集成会自适应处理源系统的字段变更，具体策略是：如果源系统的字段在MaxCompute目标表不存在，由数据集成自动添加字段；如果目标表的字段在源系统中不存在，数据集成自动填充NULL。

## 命名规范 {#section_okm_mso_arm .section}

-   表命名规范

    表命名规则：\{层次\}\{源系统表名\}\{保留位/delta与否\}

    -   增量数据：\{project\_name\}.s\{源系统表名\}delta
    -   全量数据: \{project\_name\}.s\_\{源系统表名\}
    -   ODS ETL过程的临时表：\{project\_name\}.tmp\{临时表所在过程的输出表\}\{从0开始的序号\}
    -   按小时的增量表：\{project\_name\}.s\{源系统表名\}\{delta\}\_\{hh\}
    -   按小时的全量表：\{project\_name\}.s\{源系统表名\}\{hh\}
    -   当从不同源系统同步到一个project下造成表命名冲突时，后进来的表的命名需加上源系统的dbname以解决冲突。
-   字段命名规范
    -   字段默认使用源系统的字段名称。
    -   字段名与MaxCompute关键字冲突时的处理规则：在源字段名后加一个”col”，即源字段名col。
-   同步任务命名规范
    -   任务名：\{源系统表名\}\[delta\]

        **说明：** 同一project下异库同名表的任务名：\{源系统表名\}\{tddl的appname\}\[\_delta\]。

    -   任务的输出名称需与[数据存储及生命周期管理规范](#section_jvp_q65_xrn)保持一致，即为输出表的名称。

## 数据存储及生命周期管理规范 {#section_jvp_q65_xrn .section}

|数据表类型|存储方式|最长存储保留策略|
|-----|----|--------|
|ODS流水型全量表|按天分区| -   不可再生情况下永久保存。
-   日志（非常大:如一天数据大于100G）数据保留24个月。
-   自主设置是否保留历史月初数据。
-   自主设置是否保留特殊日期数据。

 |
|ODS镜像型全量表|按天分区| -   重要的业务表及需要保留历史的表视情况保存。
-   ODS全量表的默认生命周期管理为保留2天，并采用`ds=max_pt(tablename)`方式进行数据访问。

 |
|ODS增量表|按天分区|有对应的全量表则最多保留最近14天分区数据。无全量表的增量数据需永久保留。|
|ODS ETL过程临时表|按天分区|最多保留最近7天分区|
|BDSync非去重数据|按天分区|由应用通过中间层做保留历史处理，默认ODS层不保留历史。|

## 数据质量规范 {#section_wq1_kqh_6tq .section}

-   每个ODS全量表必须配置唯一性字段标识。
-   每个ODS全量表必须做分区空数据监控。
-   建议对重要表的重要枚举类型字段做枚举值变化及枚举值分布监控。
-   建议对ODS表的数据量及数据记录数设置上周同比无变化监控，用于监控源系统是否下线或者已迁移。
-   只有有监控要求的表才创建数据质量管控层，应由DataWorks的数据质量配置完成。
-   每个ODS层全表都必须要有注释。

