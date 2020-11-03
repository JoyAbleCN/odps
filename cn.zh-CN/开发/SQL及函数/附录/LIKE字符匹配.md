---
keyword: LIKE
---

# LIKE字符匹配

本文为您介绍LIKE字符匹配的使用方法。

在LIKE匹配时，`%`表示匹配任意多个字符，`_`表示匹配单个字符。

如果要匹配`%`或`_`本身，则要对其进行转义，`\\%`匹配字符`%`， `\\_`匹配字符`_`。

```
'abcd' like 'ab%' -- true
'abcd' like 'ab_' -- false
'ab_cde' like 'ab\\_c%'; -- true
```

**说明：** 目前MaxCompute SQL支持UTF-8的字符集。如果数据以其它格式编码，计算结果可能不正确。

