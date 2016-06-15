---
title: Clean Coder 答题
date: 2016-04-18
categories: programming
tags: [ngrok,network]
---

# 使用ngrok 进行内网穿透

Step1  进入 [http://ngrok.com](http://ngrok.com) ,注册账户

Step2  下载ngrok可执行文件

<!--more-->

Step3 安装 authtoken

```shell
./ngrok authtoken um7dG4WN89fFspdbDEuD_255MaHNHv1FVEP9TmV98y
```

Step4 运行 ngrok

```shell
./ngrok http 80
```

Step5 打开本地端口  [http://localhost:4040/status](http://localhost:4040/)

查看域名

