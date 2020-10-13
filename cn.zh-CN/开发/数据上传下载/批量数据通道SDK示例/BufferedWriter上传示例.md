---
keyword: [上传示例, ]
---

# BufferedWriter上传示例

本文通过代码示例向您介绍如何使用BufferedWriter接口实现数据上传。

```
// 初始化MaxCompute和tunnel的代码。
RecordWriter writer = null;
TableTunnel.UploadSession uploadSession = tunnel.createUploadSession(projectName, tableName);
try {
  int i = 0;
  // 生成TunnelBufferedWriter的实例。
  writer = uploadSession.openBufferedWriter();
  Record product = uploadSession.newRecord();
  for (String item : items) {
    product.setString("name", item);
    product.setBigint("id", i);
    // 调用write接口写入数据。
    writer.write(product);
    i += 1;
  }
} finally {
  if (writer != null) {
    // 关闭TunnelBufferedWriter
    writer.close();
  }
}
// uploadSession提交，结束上传。
uploadSession.commit();
```

