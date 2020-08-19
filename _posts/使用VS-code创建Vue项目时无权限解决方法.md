---
title: 使用VS code创建Vue项目时无权限解决方法
date: 2019-12-1 21:52:21
tags:
- vs code
- vue
- 权限
categories:
- VS code
---

![](/images/使用VScode创建Vue项目时无权限解决方法1.png)<!-- more -->

> 1. 以管理员身份运行PowerShell
> 2. 执行：get-ExecutionPolicy，回复Restricted，表示状态是禁止的
> 3. 执行：set-ExecutionPolicy RemoteSigned
> 4. 选择Y
> 5. 注意：一定要以管理员的身份运行PowerShell，不是cmd窗口！