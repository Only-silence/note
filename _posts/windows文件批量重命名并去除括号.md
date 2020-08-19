---
title: windows文件批量重命名并去除括号
date: 2020-03-23 10:35:19
tags:
- bat
- Windows
- 文件
- 重命名
categories:
- bat脚本
---

##### 批量重命名文件后会出现(1)...  ,  利用bat脚本去除括号

<!--more--> 

1、首先把照片全部选中，然后右键选择一张，选择重命名，这里注意，直接将原来照片名删除即可，不需要输入任何字符（或者输入你自己喜欢的名称），之后，照片就被重命名为这种形式了  (1).jpg，  (2).jpg，  (3).jpg，.........

2、将如下代码拷贝到任一个记事本文件里面，将记事本后缀改为.bat，注意一定是真正的后缀是.bat，有的人改完之后是.bat.txt，这样是没有效果的

```bash
@Echo Off&SetLocal ENABLEDELAYEDEXPANSION
FOR %%a in (*) do (
set "name=%%a"
set "name=!name: (=!"
set "name=!name:)=!"
ren "%%a" "!name!"
)
exit
```

