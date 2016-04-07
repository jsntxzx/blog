---
title: Hexo博客搭建记录
date: 2016-04-07
categories: programming
tags: [hexo, github, travis ci]
---

# 环境搭建

Bash环境(Linux/Unix系统), Github账号, Ruby, git, nodejs,TravisCI账号

这里以Ubuntu 14.04系统为例，进行安装依赖项：

<!--more-->

### 安装基本软件

```shell
$ sudo apt-get update
$ sudo apt-get install git curl 
```

### 安装ruby环境

​	Step1 安装RVM软件

```shell
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
$ \curl -sSL https://get.rvm.io | bash -s stable
$ exit
```

​       退出当前的终端环境，重新登陆

​       Step2 配置RVM

```shell
$ source /usr/local/rvm/scripts/rvm
$ rvm requirements
```

​       Step3 安装Ruby

```shell
$ rvm install ruby
$ rvm use ruby --default
```

### 安装nodejs环境

```shell
$ curl -sL https://deb.nodesource.com/setup_5.x | sudo -E bash -
$ apt-get install -y nodejs
```

### 网站账号注册

github: [https://github.com](https://github.com)

travis-ci : [https://travis-ci.org](https://travis-ci.org) 需要首先注册github，然后通过github账号登陆

# Hexo本地环境搭建

```shell
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
```

当安装完成后，启动本地的服务器测试是否安装成功

```shell
$ hexo server
```

![local run](http://7xsp5k.com2.z0.glb.clouddn.com/b1-0.PNG)

如果能正确显示的话，说明hexo的本地环境配置成功了

![running](http://7xsp5k.com2.z0.glb.clouddn.com/b1-2.PNG)

# Github仓库搭建

新建一个远程仓库，如图所示:

![build github repo](http://7xsp5k.com2.z0.glb.clouddn.com/b1-1.PNG)

在建立完成之后，进入上一步的blog目录

```shell
$ cd blog
$ git init
$ git remote add origin https://github.com/jsntxzx/myblog.git
```

然后将本地的内容提交

```shell
$ git add .
$ git commit -m 'init'
```

同步本地提交到远程仓库, 同步时要求输入 Github 的帐号用户名和密码进行验证。

```shell
$ git push origin master
```

根据Github官方的[文档](https://pages.github.com/) ,为我们的博客项目创建一个Github Page分支,并同步到远程仓库

```shell
$ git checkout gh-pages
$ git push origin gh-pages
```

检查Github的repo下是否会出现一个新的分支,成功出现后说明同步成功。

![new branch](http://7xsp5k.com2.z0.glb.clouddn.com/b1-3.PNG)

修改blog目录下的配置文件_config.yml, 这个需要根据个人设置进行修改

```
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: 我的博客
subtitle:
description:
author: Zx Xiang
language:
timezone:

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://jsntxzx.github.io/blog
root: /blog/
permalink: :year/:month/:day/:title/
permalink_defaults:

# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace:

# Category & Tag
default_category: uncategorized
category_map:
tag_map:

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: landscape

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/jsntxzx/blog.git
  branch: gh-pages

```

主要注意修改的是**URL** 和 **deploy** 部分，保证最后部署的分支和URL是正确的。修改完成后进行自动化部署

```shell
$ npm install hexo-deployer-git --save
$ hexo generate --deploy
```

完成部署以后进入`http://jsntxzx.github.io/blog/` 进行访问，达到与Hexo之前本地部署一样的效果。



# Travis CI 配置

完成以上几步工作后，其实就已经可以进行博客的写作了，然而当需要在其它设备上进行博客写作的话，仍然需要将整个github的repo clone下来，配置nodejs环境——配置hexo环境——编写md文件——提交发布到两个不同的分支，这整个流程就太复杂了。此时就需要Travis-CI的接入来摆脱 hexo 的操作，让写作发布可以成为一件随时随地不受设备限制而愉快进行的事。



登录 Travis-CI，绑定 Github 帐号。完成绑定后，通过首页的Account 进入仓库列表，进行仓库同步和授权操作。

![travis init](http://7xsp5k.com2.z0.glb.clouddn.com/b1-4.PNG)



Travis-CI的作用是将原本需要在本地完成的类似 Hexo generate，deploy 的操作，全部移交至Travis-CI虚拟机的任务来自动处理。只需要在项目的目录中添加一个`.travis.yml` 文件，虚拟机自动执行指定的命令就可以了。

Travis-CI的执行流程如图所示:

![work flow](http://7xsp5k.com2.z0.glb.clouddn.com/b1-5.png)

配置Travis CI对repo的写权限,主要需要一下步骤:

Step1 进入项目目录，创建配置文件

```shell
$ cd blog
$ touch .travis.yml
```

Step2 创建密钥，并测试ssh-agent的可用性

```shell
$ ssh-keygen -t rsa -C "your_email@example.com"
$ eval "$(ssh-agent)" 
```

Step3 添加生成好的 SSH key 到 ssh-agent

```shell
$ ssh-add ~/.ssh/id_rsa
```

Step4 将` ~/.ssh/id_rsa.pub`的内容复制到 github 上项目的 deploy key 中

![add deploy key](http://7xsp5k.com2.z0.glb.clouddn.com/b1-6.PNG)

Step5 将本地项目对应的中心仓库 URL 更换为 SSH 格式

```shell
$ cd blog # 进入项目目录
$ git config --local --list # 查看当前中心仓库的 URL, 此时应该为 https 格式

...
remote.origin.url=https://github.com/jsntxzx/blog.git
...

$ git config --local remote.origin.url git@github.com:jsntxzx/blog.git   # 将中心仓库 URL 更换为 SSH 格式

```

Step6 修改项目`_config.yml`文件中`deploy`配置段`repo`为 SSH 格式 URL

```shell
deploy:
  type: git
  repo: git@github.com:jsntxzx/blog.git
  branch: gh-pages
```

Step7 私钥加密，首先安装travis-ci命令行工具，然后测试登录,登录成功后，加密密钥，这时会生成加密后的密钥文件 id_rsa.enc。同时会将相关的指令插入`.travis.yml`文件中。

```shell
$ gem install travis
$ travis login --auto
$ travis encrypt-file ~/.ssh/id_rsa --add
```

Step8 设置ssh登录的配置。创建.travis目录，并在该目录下创建一个ssh_config的文件，在文件中写入如下内容

```shell
Host github.com
	User git
	StrictHostKeyChecking no
	IdentityFile ~/.ssh/id_rsa
	IdentitiesOnly yes
```

Step9 编写`.travis.yml`配置文件

```shell
language: node_js

sudo: false

branches:
  only:
  - master

before_install:
- mkdir -pv ~/.ssh/
- openssl aes-256-cbc -K $encrypted_f739449d5e95_key -iv $encrypted_f739449d5e95_iv
  -in id_rsa.enc -out ~/.ssh/id_rsa -d
- chmod 600 ~/.ssh/id_rsa
- eval $(ssh-agent)
- ssh-add ~/.ssh/id_rsa
- cp .travis/ssh_config ~/.ssh/config
- git config --global user.name 'jsntxzx-travis'
- git config --global user.email 'jsntxzx@sina.com'

install:
- npm install hexo-cli -g
- npm install

script:
- hexo clean
- hexo g
- hexo d
```

**注意：以上的配置文件中的相关配置内容需要针对各自的项目进行修改。不可直接拷贝。**

完成以上工作后，将master修改push

```shell
$ git add .
$ git commit -m "site publish"
```

进入Travis-CI网站，可以看到最新Build的结果:

![build result](http://7xsp5k.com2.z0.glb.clouddn.com/b1-7.PNG)

如果Build成功，这此时进入`http://jsntxzx.github.io/blog/` 进行访问，内容会得到更新。

























