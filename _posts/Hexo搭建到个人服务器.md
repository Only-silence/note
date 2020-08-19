---
title: Hexo搭建到个人服务器
date: 2019-12-21 03:08:37
tags:
- Linux
- Hexo
- 服务器
categories:
- Linux
---

#### 前言

​		说实话，这玩意整了两天我才整好，网上大多数都是将`Hexo`搭建到`GitHub`上，找了很长时间才找到几个搭建在个人服务器上的，但是都不是很详细，所以我汇总了一下，得出以下经验。

<!-- more -->

#### 环境：

- 阿里云轻量级服务器
- 宝塔面板
- nginx
- nod + npm + git

#### 1. 安装所需环境

![](/images/Hexo搭建到个人服务器1.png)

#### 2. 添加站点并非设置默认路径

![](/images/Hexo搭建到个人服务器2.png)

#### 3. 创建git仓库

ssh远程连接服务器，在服务器默认路径下创建`git`仓库    `hexo.git`  (我的默认路径 `/www/wwwroot/`)

```
mkdir hexo.git

cd hexo.git

git init --bare
```

然后回到 `/www/wwwroot/` 默认路径下，创建一个文件夹 `hexo `并赋予权限，这就是以后网站存放目录

```
mkdir hexo

chmod 0755 hexo

chown root:root -R /www/wwwroot/hexo
```

再创建一个  tmp  目录，后面要用到

```
mkdir tmp
```

#### 4. git配置

```
vim hexo.git/hooks/post-receive
```

向里面写入（ i 键激活写入）：

```
#!/bin/bash -l

GIT_REPO=/www/wwwroot/hexo.git                      # 触发 hook

TMP_GIT_CLONE=/www/wwwroot/tmp                # 存在 /tmp 下

PUBLIC_WWW=/www/wwwroot/hexo                 # 展示网站的目录

rm -rf ${TMP_GIT_CLONE}                  # 删除之前内容

git clone $GIT_REPO $TMP_GIT_CLONE       # 将 Git 仓库上传的内容复制到/tmp

rm -rf ${PUBLIC_WWW}                   # 删除展示网站的目录的全部内容

cp -rf ${TMP_GIT_CLONE} ${PUBLIC_WWW}  # 将/tmp所有内容复制到网站目录
```

写入完成后保存退出即可（ : 键激活输入命令,然后输入 wq 保存并退出 ）

更改脚本权限

```
chmod +x post-receive
```

#### 5. nginx配置

```
cd /etc/nginx/sites-available

vim default
```

删除内容并写入：

```
server {
    listen 80;
    listen [::]:80;
    root /www/wwwroot/hexo;
    index index.html index.htm index.nginx-debian.html;
    server_name 你的域名;#没有域名可不用填写
    location ~* ^.+\.(ico|gif|jpg|jpeg|png)$ {
        root /www/wwwroot/hexo;
        access_log   off;
        expires      1d;
    }
    location ~* ^.+\.(css|js|txt|xml|swf|wav)$ {
        root /www/wwwroot/hexo;
        access_log   off;
        expires      10m;
    }
    location / {
        root /www/wwwroot/hexo;
        if (-f $request_filename) {
            rewrite ^/(.*)$  /$1 break;
        }
    }
}
```

然后保存并退出，使用命令重启`nginx`

```
service nginx restart
```

#### 6. 本地环境搭建

环境：

- win10
- node + npm + git

##### 安装`Hexo`:

`npm install -g hexo-cli`

安装完后新建文件夹（例 `E：hexo`），然后执行以下命令

```
hexo init <folder>

cd <folder>

npm install
```

本地`hexo`搭建完成，运行`hexo s`命令即可访问本地项目 http://localhost:4000

##### 本地`git`环境搭建

1. 安装`git`

2. 生成`ssh`认证，执行如下命令

   ```
   git config --global user.name "yourname"
   
   git config --global user.email “youremail”
   
   ssh-keygen -t rsa -C "youremail"
   
   git config --global core.autocrlf false  // 禁用自动转换
   ```

   最后获取到的`ssh`认证在`C:\Users\yourname\.ssh`中

3. 在本地建立`ssh`信任关系：

   ```
   ssh-copy-id -i C:/Users/yourname/.ssh/id_rsa.pub root@server_ip
   
   ssh root@server_ip // 测试能否登录
   ```

   打开本地`hexo`文件夹，在`_config.yml`中更改：

   ```
   deploy:
   
   type: git                      *# 设置上传模块为 Git*
   
   repo: root@sever_ip:/www/wwwroot/hexo.git  *# 连接到服务器*
   
   branch: master                 *# 存储在 master 分支（主分支）*
   ```

   保存即可



#### 7. 基本命令

`cd hexo`进入本地环境文件夹

执行`hexo new '文章标题'`创建新文章

- `title`是标题，`date`是编写时间，`tags`是标签，`categories`是分类且一篇文章只有一个分类

- 例如本文格式：

  ``` 1232224
  title: Hexo搭建到个人服务器
  date: 2019-12-21 03:08:37
  tags:
  - Linux
  - Hexo
  - 服务器
  categories:
  - Linux
  ```

第一次运行执行`hexo clean && hexo g && hexo d`即可将写好的博客上传至服务器

以后可直接`hexo g && hexo d`

##### Hexo常用命令

> ```bash
> hexo g # 是hexo generate的简写形式，生成静态文件到public文件夹
> ```

> ```bash
> hexo d # 是hexo deploy的简写形式，部署静态文件到设定的仓库
> ```

> ```bash
> hexo clean && hexo g && hexo d # 常用的组合命令，清除缓存，生成静态文件，并部署到指定仓库
> ```



#### 注意事项：

- hexo目录下可以新建其它文件夹，但是不可再执行`hexo clear`命令，否则会报404错误，解决方法是删除新建的文件夹后执行`hexo clean && hexo g && hexo d`即可。

- 无权限rm 

  ![](/images/Hexo搭建到个人服务器3.png)

  解决方法是在 `/www/wwwroot/`下运行：

  > chmod 777 .

  注：要用`root`权限

  

#### 8. 参考链接

- https://hexo.io/zh-cn/docs/
- https://www.cnblogs.com/luoshuitianyi/p/10333928.html
- https://blog.csdn.net/qq_35561857/article/details/81590953
- https://www.jianshu.com/p/196773379a78
- https://www.jianshu.com/p/7d8f6cfa6b43