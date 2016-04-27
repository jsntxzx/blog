---
title: 使用ShareX和QiniuMonitor构建markdown图床
date: 2016-04-27
categories: other
---

# 使用ShareX和QiniuMonitor构建markdown图床

## ShareX

ShareX是一款开源的截图软件，提供较强的截图功能，其官网是[sharex](http://getsharex.com)

### 热键设置

![hotkey](http://7xsp5k.com1.z0.glb.clouddn.com/ShareX_2016-04-27_08-58-02.png)

<!--more-->

### 目标文件夹设置

![](http://7xsp5k.com1.z0.glb.clouddn.com/ShareX_2016-04-27_09-02-16.png)

## QiniuMonitor

QiniuMonitor由C# Winform开发，通过监控文件夹的创建事件，把新增的文件上传到设置好的七牛云存储空间中。

项目介绍见: [project homepage](https://jsntxzx.github.io/QiniuMonitor)

按照流程设置好必要参数后，点击开始监控。则所有的截图会自动上传到七牛云存储，并且其外链将拷贝到剪切板。



### 使用流程

![usage](http://7xsp5k.com1.z0.glb.clouddn.com/2016-04-27_09-59-18.gif)