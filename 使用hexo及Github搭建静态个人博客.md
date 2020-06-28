---
title: 使用hexo及Github搭建静态个人博客
date: 2019-12-27 22:04:36
tags: [笔记,博客,GitHubPage,Hexo ]
categories: [笔记 ]
---

# 使用Github page 及 Hexo 创建个人博客
## 什么是GitHub Page
> GitHub Page，一般多用于托管个人的静态网站，所以现在很多人也用来它来搭建私人博客，也算是省去了购买服务器、域名等等一系列复杂的操作。搭建博客网站有各种各样的方法，像懂php的可以用WordPress，懂Java的可以用Jpress等等。如果你想简单和简约，那么我强烈推荐你使用Github Page。在学习以下内容之前，请先准备好GitHub账号，如果没有请自行注册。
<!--more-->
## 什么是Hexo
> Hexo是一个简单、快速、强大的基于 Github Pages的博客发布工具，支持Markdown格式，有众多优秀插件和主题。

## 为何使用GitHub Page搭建博客
### 优点
* GitHub的存储库可以做为免费的服务器存储
* GitHub Page提供免费的用于域名解析的CNAME地址
* GitHub服务器位于国外，可以免去麻烦冗长的备案手续和时间
* Hexo有丰富的主题和插件系统，界面美观功能实用
* 全静态页面，博客安全风险相对较低

### 缺点
* GitHub服务器位于国外，所以访问速度可能会相对较慢
* 全博客为静态页面，更新文章的过程较为复杂

## 准备阶段
* [GitHub](https://github.com/)账号
* [Git](https://git-scm.com/)客户端（git bash）
* [npm](http://nodejs.cn/)并[配置环境变量](https://jingyan.baidu.com/article/e4511cf38c05092b845eaf9b.html)
* [Hexo](https://hexo.io/):使用npm进行安装，具体内容见下文

## 搭建配置过程
### 本地使用git bash创建sshkey
* 在本地任意位置右击鼠标（已安装git bash），选择`git bash here`,在打开的终端中输入   
```shell
cd ~/.ssh    #检查本机已存在的ssh密钥
```
* 创建sshkey
```shell
ssh-keygen -t rsa -C "YourEmailAddress"
```
* 将`.ssh\id_rsa.pub`文件中的内容复制，粘贴到GitHub账号的`Settings>SSH and GPG keys>SSH keys>NEW SSH KEYS`,粘贴到key文本框中保存![key值示例](https://i.jpg.dog/img/a8ae11c5d92c993cbbc46c48aae5b360.png)
* 在gitbash中输入如下命令：
```shell
$ ssh -T git@github.com # 不用修改
```
若提示`Are you sure you want to continue connecting (yes/no)?`，则输入yes,会出现如下提示：
> Hi [Your Username]! You've successfully authenticated, but GitHub does not provide shell access.

* 进行github账号信息的配置
```shell
$ git config --global user.name "username"// 你的github用户名，非昵称
$ git config --global user.email  "xxx@xx.com"// 填写你的github注册邮箱
```

### 创建博客存储库，并设置
* 在github中创建任意名称存储库（自己有域名的话），若无域名，推荐使用`username.github.io`格式命名，方便后期进行访问
* 在存储库的setting页面的GitHub Page页面进行如下设置,Source项选择`master分支`，若自己有域名则设置Custom domain项为要解析到的域名，https选项推荐勾选，并点击“SAVE”按钮保存。效果如下图：![GitHubpage设置](https://i.jpg.dog/img/a795d514793b98064049870b81461429.png)
* 在域名提供商的域名解析处，将GitHubPage提供的cname域名解析到刚才输入的域名，如下图:![376958d20da98a43eedd253dd8215851.png](https://i.jpg.dog/img/376958d20da98a43eedd253dd8215851.png)

### 使用npm安装Hexo
* 在git bash 中使用如下命令安装Hexo
```shell
$ npm install -g hexo
```
* 如果报错提示command not found ,则在npm的安装目录中搜索hexo所在，并添加环境变量
* 有时git bash在打开后会无法使用新添加的环境变量，重新打开git bash即可

### 初始化Hexo
* 在任意位置建立任意名称文件夹，作为Hexo的工作目录
* 打开该目录（该目录必须为空），执行如下命令进行初始化
```shell
$ hexo init
```
* 初始化过程中，git bash会自动从GitHub下载hexo项目并部署

### 对内容进行修改
* 更改工作目录中的_config.yml文件，修改前请做好备份
* 根据自己的具体信息配置文件中的内容，具体配置文件有注释
* 配置Deployment信息,方便可以直接通过Hexo命令进行代码的提交
```xml
deploy:
    type:'git'
    repository: git@github.com:username/username.github.io.git
    branch: master
```
### 关于主题
* Hexo官方提供了在线[主题商店](https://hexo.io/themes/)，可以直接访问下载。以[hexo-theme-ayer](https://shen-yu.gitee.io/)主题为例,github地址[https://github.com/Shen-Yu/hexo-theme-ayer](https://github.com/Shen-Yu/hexo-theme-ayer),执行如下命令：
```shell
git clone https://github.com/Shen-Yu/hexo-theme-ayer theme/ayer
```
* 修改_config.yml中的theme的值为主题文件夹的名称
* 注意修改主题文件夹下的_config.yml值（根据README.md中的指引修改）

## 使用指南_操作命令
### Hexo常用命令
```shell
hexo new "postName" # 新建文章，且自动添加初始化文件头
hexo new page "pageName"    # 新建页面
hexo generate   # 生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，可通过-p 临时重定义'ctrl + c'关闭server，端口间应用数据互相独立）
hexo deploy #部署到GitHub
hexo help   # 查看帮助
hexo version    #查看Hexo的版本
```
### 常用命令简写
```shell
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```
### 常用组合命令
```shell
hexo s -g   #生成并本地预览
hexo d -g   #生成并上传
```
## 使用指南_文章格式
* 文章可以直接使用hexo new "postName"生成到source/_post/文件夹下
* 生成的文件会自带简述格式，也可自行建立文件进行编辑，简述格式如下
```markdown
---
title: postName #文章页面上的显示名称
date: 2013-12-02 15:30:16 #文章生成时间，可以任意修改
categories: 默认分类 #分类
tags: [tag1,tag2,tag3] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: 附加一段文章摘要，字数最好在140字以内，会出现在meta的description里面
---

以下是正文
```
* 文章列表指定缩略截止（`<!--more-->`）
```markdown
# 使用Github page 及 Hexo 创建个人博客
## 什么是GitHub Page
> GitHub Page，一般多用于托管个人的静态网站，所以现在很多人也用来它来搭建私人博客，也算是省去了购买服务器、域名等等一系列复杂的操作。搭建博客网站有各种各样的方法，像懂php的可以用WordPress，懂Java的可以用Jpress等等。如果你想简单和简约，那么我强烈推荐你使用Github Page。在学习以下内容之前，请先准备好GitHub账号，如果没有请自行注册。
<!--more-->
## 什么是Hexo
> Hexo是一个简单、快速、强大的基于 Github Pages的博客发布工具，支持Markdown格式，有众多优秀插件和主题。

## 为何使用GitHub Page搭建博客
```
* 最终效果：
![ab87b45249cfd20d5a9c225f0fc28717.png](https://i.jpg.dog/img/ab87b45249cfd20d5a9c225f0fc28717.png)


## 本博客最终效果预览
![aa005f434285514ddc7bedb79fd51dc4.png](https://i.jpg.dog/img/aa005f434285514ddc7bedb79fd51dc4.png)