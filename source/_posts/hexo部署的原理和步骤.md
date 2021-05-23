---
title: hexo部署的原理和步骤
categories:
  - null
tags:
  - null
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
article: {
  title:
  date:
  tags:
  categories:
  content:
  ...
}
```

- 第二步，根据上面生成的JSON对象生成 HTML 页面。

对于我们来说，清楚了上面hexo将Markdown转HTML页面的过程后，我们就很容易理解hexo 在执行不同命令时它都在做什么事儿了。

下面我们再来看看**hexo**的组成，它由三部分组成: **hexo-cli**、**hexo-core**以及**hexo plugs**。在这三部分中最核心的是hexo-core模块，它的作用就是执行上面讲的两步转换，从而生成目标文件；hexo-cli为我们供了一些非常方便的命令。当我们敲入命令时，它会根据命令调用不同的模块；hexo plugin是hexo的扩展，当hexo本身不能完成某项任务时，它允许你自己开发一个插件来完成。当然你也可以使用其它人写好的插件。

##### 2) 这里我们来分析一下 Hexo 每次部署的流程

1. hexo g：生成静态文件。将我们的数据和界面相结合生成静态文件的过程：会遍历主题文件中的 source 文件夹（js、css、img 等静态资源），然后建立索引，然后根据索引生成 pubild 文件夹中，此时的 publid 文件是由 html、 js、css、img 建立的纯静态文件可以通过 index.html 作为入口访问你的博客。
2. hexo d：部署文件。部署主要是根据在 _config.yml 中配置的 git 仓库或者 coding 的地址，将 public 文件上传至 github 或者 coding 中。然后再根据上面的 github 提供的 pages 服务呈现出页面。当然你也可以直接将你生成的 public 文件上传至你自己的服务器上。
3. 在node_modules中有一系列的文件用于对hexo中的各类页面进行默认的渲染，如果要启动个性化主页，需要删除hexo-generator-index,同时，将主题目录下的source目录作为你个性化页面的根目录。



## 二、模板引擎

表现是一个博客的个性。

#### 1. hexo的模板引擎

> ##### 模板引擎的作用，就是将界面与数据分离。最简单的原理是将模板内容中指定的地方替换成数据，实现业务代码与逻辑代码分离。

- 我们可以注意到，在 Hexo 中，source 文件夹和 themes 文件夹是在同级的，我们就可以将 source 文件夹理解为数据库，而主题文件夹相当于 界面。然后我们 hexo g 就将我们的数据和界面相结合生成静态文件 public。

- hexo默认的是使用ejs，同类型的东西还有很多，比如jade，swig。我选用主题是用jade的。
  hexo首先会解析md文件，然后根据layout判断布局类型，再调用其他的文件，这样每一块的内容都是独立的，提高代码的复用性。最终会生成一个html页面。

- jade采用缩进语法格式，和python比较类似，看上去也很舒服，我比较喜欢这种风格。在hexo中使用jade需要安装相应的模块，否则无法使用。

- 模板文件在 layout 文件夹下，layout 文件文档结构如下：

```
.
├── _custom                           // 通用布局
├── _layout.swig                      // 默认布局布局
├── _macro                            // 插件模板
├── _partials                         // 局部布局
├── _scripts                          // script模板
├── _third-party                      // 第三方插件模板
├── archive.swig                      // 归档模板
├── category.swig                     // 分类模板
├── index.swig                        // 首页模板
├── page.swig                         // 其他模板
├── photo.swig                        // 照片模板（自定义）
├── post.swig                         // 文章模板
├── schedule.swig                     // 归档模板
└── tag.swig                          // 标签模板
```

> ##### 每个模板都默认使用layout布局，您可在文章的前置申明中指定其他布局，比如“post”或者“page”或是设为false来关闭布局功能（如果不填默认是post，这个在_config.yml中可以设置默认值），您甚至可在布局中再使用其他布局来建立嵌套布局。

  - 在我们新建页面或者新建文章的使用可以选定我们使用的模板。hexo new [layout] <title>就会使用对应的模板。

  -  其中 _layout.swig 是通用模板，里面引入了 head、footer 等公共组件，然后在其他的模板中会引入这个 _layout.swig 通用模板，比如 post.swig 模板

#### 2. 数据的填充

数据的填充主要是 hexo -g 的时候将数据传递给 swig 模板，然后再由 swig 模板填充到 HTML 中。

#### 3. 使用yaml编写的配置文件

- yaml是专门用来写配置文件的语言。它用首行缩进表示层级关系，便于读写理解。

- 配置文件一般用来对所需环境进行设置。hexo中涉及到两个配置文件，一个是位于主目录下的，另一个是位于主题目录下的。

- 通常主目录下的配置文件用于对全站的配置，比如站点的基本信息，文章的布局，写作的格式，部署到github上的参数等等。

- 而主题目录下的配置文件用于对该主题的配置，比如站点导航栏的设置，一些插件的设置等。

Hexo 的配置文件 _config.yml 使用 yml语法 。例如博客的名字、副标题等等之类。这些数据项组织在 config 对象中。可以数字、字符串、对象、数组，

#### 4. 配置文件中数据的使用

如果要在模板中使用某个具体的值，比如字符串、数字、逻辑变量或者对象的某个成员，可以在主题的模板文件 swig 中直接使用：

```text
{% block title %} {{ page.title }} | {{ config.title }} {% endblock %}
```

#### 5. 使用markdown编写的博客文章

之所以选择hexo做博客一个原因就是它支持markdown。用markdown写文章感觉特别爽，只需要记住简单的几个语法，而且可以把全部的注意力放在文字本身上，而不用去过多的关注排版。



# 三、**hexo发布页面**

我们可以使用`hexo d`命令将生成的目标文件发布出来，但在使用它之前，你需要在站节点的_config.yml文件中配置发布的方法。我们举个例子：

```text
deploy:
- type: git #leancloud_counter_security_sync #git
  repo: git@github.com:avdance/avdance.github.io.git
```

当我们执行`hexo d`命令时，它会调用 hexo 中的 `hexo-deployer-git` 插件。在该插件内部会启动一个进程调用`git`命令，从而将生成的html等代码上传到 github上。

这里可能有些同学会有疑问，为什么上传到github上就算是发布了呢？这是因为github为我们提供了免费的个人博客空间。只要你在github上创建一个`用户名.github.io`的项目，github就会自动将这个项目中的文件发布出来。

当然你也可以采用传输的方式，自己购买台云主机，然后在云主机上用ngnix、nodejs等搭建一个Web服务，最终将页面发布出来。



## 四、**hexo的常用命令**

**hexo** 提供了几个常用命令，如`hexo clean`、`hexo g`、`hexo s`等等。下面我们分别看一下这几个命令的具体作用是什么：

- hexo clean: 删除 hexo 生成的所有文档。当我们执行这个命令后，你会发现public目录被删除了。
- hexo g: 根据 source 目录中的文件生成html等可以发布的文件。
- hexo s: 在本地起动 **http** 服务，将生成的 html 等输出文件布署到本地服务器上。
- hexo d: 将生成的html代码推送到 github 上



## 五、Hexo 中的变量

#### 1、Hexo 中的变量

Hexo 提供了很多的变量，比如我们上面使用的 page 变量，还有 site 变量等，这些都是Hexo 提供的，我们可以拿来直接使用的，常用的变量有：

- site：对应整个网站的变量，一般会用到 site.posts.length 制作分页器。

1. site.posts 所有文章
2. site.pages 所有分页
3. site.categories 所有分类
4. site.tags 所有标签

- page：存放当前页面的信息，例如我在 index.ejs 中使用 page.posts 获取了当前页面的所有文章而不是使用 site.posts。
- config：config 变量我们在主目录下配置文件 _config.yml 中保存的信息。
- theme：theme 变量是我们在主题目录下配置文件 _config.yml 中保存的信息。
- path：当前页面的路径（不含根路径）。
- url：页面完整网址。

#### 2、页面变量

Page(page) 这里指的是`hexo new page`创建的那个页面

- page.title：文章标题
- [page.date](https://link.zhihu.com/?target=http%3A//page.date)：文章建立日期
- page.updated：文章更新日期
- page.comments：留言是否开启
- page.layout：布局名称
- page.content：文章完整内容
- page.excerpt：文章摘要
- page.more：除了摘要的其他内容
- page.source：文章原始路劲
- page.full_source：文章完整原始路径
- page.path：文章网址（不含根路径），通常在主题中使用url_for(page.path)
- page.permalink：文章永久网址
- page.prev：上一篇文章，如果此为第一篇文章则为null
- [page.next](https://link.zhihu.com/?target=http%3A//page.next)：下一篇文章，如果此为最后一篇文章则为null
- page.raw：文章原始内容
- [page.photos](https://link.zhihu.com/?target=http%3A//page.photos)：文章的照片（用于相册）
- [page.link](https://link.zhihu.com/?target=http%3A//page.link)：文章的外链（用于链接文章）

#### 3、Post(post) 变量

这里指的是文章页面，与page布局相同，添加如下变量：

- page.pulished：文章非草稿为true
- page.categories：文章分类
- page.tags：文章标签

#### 4、首页(index)

- page.per_page：每一页显示的文章数
- [page.total](https://link.zhihu.com/?target=http%3A//page.total)：文章数量
- page.current：当前页码
- page.current_url：当前页的URL
- page.posts：当前页的文章
- page.prev：前一页页码，如果为第一页，该值为0
- page.prev_link：前一页URL，如果为第一页，则为''
- [page.next](https://link.zhihu.com/?target=http%3A//page.next)：后一页页码，如果为最后一页，则为0
- page.next_link：后一页URL，如果为最后一页，则为''
- page.path：当前页网址（不含根路径），通常在主题中使用url_for(page.path)

#### 5、归档页(archive)

与index布局相同，但是新增如下变量：

- `archive` 为true
- `year` 归档年份（4位）
- `month` 归档月份（不包含0）
