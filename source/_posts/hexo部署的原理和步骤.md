---
title: hexo部署的原理和步骤
categories:
  - blog
  - web前端
tags:
  - hexo
date: 2021-05-23 09:48:29
update: 2021-05-23 09:48:29
---


> #### Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
<!-- more -->



# hexo部署的原理和步骤

---


## 本文将从以下几个方面尝试分析。

- hexo原理
  - hexo的文件夹结构
  - hexo的工作原理
  
- hexo模板引擎
  - hexo的模板引擎
  - 数据填充
  - 使用yaml编写的配置文件
  - 配置文件中数据的使用
  - 使用markdown编写的博客文章
  
- hexo发布

- hexo常用命令

- hexo变量

  - hexo变量

  - 页面变量

  - Post(post) 变量

  -  首页(index)
  
  -  归档页(archive)
  
  

## 一、原理

内容是一个博客的灵魂。



#### 1. hexo的文件夹结构

├── _config.yml
├── db.json
├── node_modules
├── package.json
├── public
├── scaffolds
├── source #所有文章文件放在这里
└── themes #主题文件夹

- **_config.yml**：该文件是hexo 的`站`级配置文件。所谓站级配置文件是指，对整个站点起效果的配置文件。

- **source目录**：该目录存放源文件。即用户编写的博文都放在该目录下。在该目录下又有几个子目录我们来分别看一下。

  - **_post**：用于存放博文，基本上每篇文章都是由Markdown语法编写的。

  - **tags**：存放tag 的文件。hexo中的tags是自动生成的，所以我们不用手动修改tags目录下的index.md文件，在发布时它会自动生成。
  - **categories**：存放**分类**。它与tags是类似的，也是自动生成的，所以不需要我们手工修改。

- **themes目录**：该目录用于存放主题，目前hexo中最热门的主题就是**next**了。最近的 next release 版本是 7.8 。

- **public目录**：该目录存放hexo转出的文件，如html、css、js等。

- **scaffolds目录**：它里面存放了一些**“脚手架”**程序，用于生成模板页面，如执行`hexo new "title"`时，就会生成一个Markdown文件模板。

- **db.json** ： 缓存文件

- **node_modules**：  安装的插件以及hexo所需的一些node.js模块。

- **package.json**： 应用程序信息，配置hexo运行需要的js包。

  

#### 2. Hexo 的工作原理

##### 1) hexo最基本的功能是将Markdown程序转成HTML页面，而这个转换并不是一次完成的，要经历两步:

- 第一步，将Markdown翻译成下面格式的JSON对象：

```text
article: &#023
  title:
  date:
  tags:
  categories:
  content:
  ...
&#025
```
