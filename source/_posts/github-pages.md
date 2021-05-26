---
title: github pages
categories:
  - web前端
tags:
  - git hub pages
  - github
date: 2021-05-22 22:22:03
update: 2021-05-22 22:22:03  
---



在用github和hexo搭建个人博客时遇到github pages。github pages是什么呢？它就是托管在github上的静态网页。建立时注意事项：

1. 仓库名和用户名相同
2. 在仓库的setting找到page，把分支改为branch分支就行l

<!-- more -->



# github pages



### 1、什么是github pages

1. 什么是github pages
   github是项目托管网站，列出了项目的源文件，所以github 有一个pages功能，可以自定义主页，用来代替默认的列出源列表的这个页面

> 所以，github Pages可以被认为是用户编写的、托管在github上的静态网页。

2. 下面是GitHub Pages 官方文档:

https://pages.github.com/
http://help.github.com/pages

3. GitHub提供两种类型的主页(https://help.github.com/articles/user-organization-and-project-pages):

   1. 个人或组织主页 - 页面内容位于 master 下
   2. 项目主页 - 页面内容位于每个项目的master下

   我们创建的博客属于个人页面（也可以创建为项目主页，不过默认的域名不一样，个人理解）

   

### 2、怎么使用github pages

1. 使用个人或组织页面
   使用个人或组织页面，需要先创建一个和你的账号同名的仓库，比我我的github账号是sunshine940326，那么我需要创建一个名为sunshine940326.github.io的repo，然后在master上提交你的项目代码，这样就可以通过网址：http://sunshine940326.github.io来访问我的个人博客。

2. 使用项目主页的方法如下

   1. 设置的方法很简单，只需要在你项目的右上角点击setting

   2. 找到下方的pages，将默认的none改成master分支
   3. 点击保存，之后就可以在github pages后面看到你的项目链接了，你可以直接通过这个链接查看你master分支中代码的html内容

   > #### user pages只有一个, project pages可以有多个, 对于个人博客而言, 两种方式都可以.如果用户申请了自己的域名, 还可以使用CNAME文件自定义domain name, 这样访问你的域名就自动访问到github上的页面. 用户也可以自定义404页面.

