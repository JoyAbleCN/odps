---
keyword: [DataWorks上使用PyODPS, DataWorks, PyODPS]
---

# 在DataWorks上使用PyODPS

本文为您介绍如何在DataWorks上使用PyODPS和使用限制。

使用示例请参见[PyODPS节点实现结巴中文分词]()。

## 新建PyODPS节点

在工作流节点中会包含PYODPS类型节点，新建工作流节点即可新建PyODPS节点。

![新建节点](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8451330061/p11645.png)

## ODPS入口

DataWorks的PyODPS节点中，将会包含一个全局变量`odps`或者`o` ，即为ODPS入口。您不需要手动定义ODPS入口。

```
print(o.exist_table('pyodps_iris'))
```

## 执行SQL

您可以在PyODPS节点中执行SQL，详情请参见[执行SQL文档](/intl.zh-CN/开发/PyODPS/基本操作/SQL.md) 。

Dataworks上默认未开启instance tunnel，即instance.open\_reader默认使用Result接口（最多一万条记录）。您可以通过reader.count获取记录数。如果您需要迭代获取全部数据，则需要关闭limit限制。您可以通过下列语句在全局范围内打开Instance Tunnel并关闭limit限制。

```
options.tunnel.use_instance_tunnel = True
options.tunnel.limit_instance_tunnel = False  # 关闭limit限制，读取全部数据。

with instance.open_reader() as reader:
    # 通过instance tunnel可读取全部数据。
```

您也可以通过在open\_reader上添加`tunnel=True`，实现仅对本次open\_reader开启instance tunnel。同时，您还可以添加 `limit=False`，实现仅对本次关闭limit限制。

```
with instance.open_reader(tunnel=True, limit=False) as reader:
# 本次open_reader使用instance tunnel接口，且能读取全部数据。
```

**说明：** 若您未开启instance tunnel，可能导致获取数据格式错误，解决方法请参见[Python SDK]()。

## DataFrame

-   执行

    在DataWorks的环境里，[DataFrame](/intl.zh-CN/开发/PyODPS/DataFrame/快速入门.md)的执行需要显式调用[立即执行的方法（如execute，head等）](/intl.zh-CN/开发/PyODPS/DataFrame/执行.md) 。

    ```
    from odps.df import DataFrame
    iris = DataFrame(o.get_table('pyodps_iris'))
    for record in iris[iris.sepal_width < 3].execute():  # 调用立即执行的方法，处理每条record。
    ```

    如果您需要在print时调用立即执行，需要开启`options.interactive` 。

    ```
    from odps import options
    from odps.df import DataFrame
    options.interactive = True  # 在开始处打开开关。
    iris = DataFrame(o.get_table('pyodps_iris'))
    print(iris.sepal_width.sum())  # print时会立即执行。
    ```

-   打印详细信息

    通过设置`options.verbose`选项。在DataWorks上，默认已经处于打开状态，运行过程会打印Logview等详细过程。


## 获取调度参数

与DataWorks中的SQL节点不同，为了避免影响代码，PyODPS节点不会在代码中替换 $\{param\_name\}这样的字符串，而是在执行代码前，在全局变量中增加一个名为`args`的dict，调度参数可以在此获取。例如，在**节点基本属性** \> **参数**中设置`ds=${yyyymmdd}`，则可以通过以下方式在代码中获取该参数。

```
print('ds=' + args['ds'])
ds=20161116
```

**说明：** 如果您需要获取名为`ds=${yyyymmdd}`的分区，则可以使用如下方法。

```
o.get_table('table_name').get_partition('ds=' + args['ds'])
```

## 使用限制

-   PyODPS节点底层的Python版本为2.7。
-   PyODPS节点获取本地处理的数据不能超过50MB，节点运行时占用内存不能超过1G，否则节点任务会被系统中止。请避免在PyODPS任务中写额外的Python数据处理代码。
-   在DataWorks上编写代码并进行调试效率较低，为提升运行效率，建议本地安装IDE进行代码开发。
-   在DataWorks上使用PyODPS时，为了防止对DataWorks的Gate Way造成压力，对内存和CPU都有限制，该限制由DataWorks统一管理。如果您发现有**Got killed**报错，即表名内存使用超限，进程被中止。因此，请尽量避免本地的数据操作。通过PyODPS发起的SQL和DataFrame任务（除to\_pandas外）不受此限制。
-   由于缺少matplotlib等包，如下功能可能受限：
    -   DataFrame的plot函数。
    -   DataFrame自定义函数需要提交到MaxCompute执行。由于Python沙箱限制，第三方库只支持所有的纯粹Python库以及Numpy，因此不能直接使用Pandas。
    -   DataWorks中执行的非自定义函数代码可以使用平台预装的Numpy和Pandas。其他带有二进制代码的三方包不被支持。
-   由于兼容性原因，在DataWorks中，options.tunnel.use\_instance\_tunnel默认设置为False。如果需要全局开启instance tunnel，需要手动将该值设置为True。
-   由于实现的原因，Python的atexit包不被支持，请使用try-finally结构实现相关功能。

