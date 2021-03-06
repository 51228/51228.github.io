---
title: git冲突合并
categories:
  - 知识点
  - git
tags:
  - git
  - 冲突解决
  - merge
date: 2021-06-20 11:32:30
update: 2021-06-20 11:32:30 
---

在使用`git push`的过程中，经常会遇到一个问题，因为文件有冲突无法push。冲突有几种表现形式？冲突的解决方式？本文就是解决以上问题。
<!-- more -->

### 目录

一. [冲突类型](#1)

1. 自动合并
2. 逻辑冲突
3. 真正的冲突
4. 冲突树

二. [Git如何记录冲突](#2)

1. git冲突是通过.git目录下的几个文件记录的：
2. 版本库暂存区中则会记录冲突文件的多个不同版本，使用`git ls-files -s`命令查看
3. 工作区的版本则可能同时包含了成功合并即冲突合并，其中冲突合并会用特殊的标记

三. [冲突的解决](#3)

1. 手工编辑完成冲突解决
2. 图形工具完成冲突解决



<a id=1>一. 冲突类型</a>

1. 自动合并
  - 修改不同的文件
  - 修改相同文件的不同区域
  - 同时更改文件名和文件内容
2. 逻辑冲突

  - 典型的逻辑冲突是一个用户修改了一个文件的文件名，而另外的用户在其他文件中引用旧的文件名，这样的合并虽然能够成功但是包含着逻辑冲突。
  - 一个用户修改了函数的返回值而另外的用户使用旧的函数的返回值，虽然能够成功但是存在逻辑冲突。

3. 冲突树

  - 如果一个用户将某个文件改名，另外一个用户把文件改为另外一个名字，当两个用户提交合并操作时，产生冲突。这种因为文件名修改造成的冲突，称为树冲突。

<a id=2>二. Git如何记录冲突</a>

1. git冲突是通过.git目录下的几个文件记录的：

  - 文件.git/MERGE_HEAD记录所合并的提交ID
  - 文件.git/MERGE_MSG记录合并失败的信息
  - 文件.git/MERGE_MODE标识合并状态
  - 可以使用`cat .git/MERGE_HEAD`,`cat .git/MERGE_MSG`,`cat .git/MERGE_MODE`来访问。

2. 版本库暂存区中则会记录冲突文件的多个不同版本，使用`git ls-files -s`命令查看：

   ![image-20210620202729446](https://gitee.com/nsaction/blog_pic/raw/master/image-20210620202729446.png)

在上面的输出中，每一行分为四个字段，前两个分别是文件的属性和SHAI哈希值。第三个字段暂存区编号，当合并冲突发生后，会用到0以上的暂存区编号。

  - 编号为1的暂存区用于保存冲突文件修改之前的副本，即冲突双方共同的祖先版本。
  - 编号为2的暂存区用于保存当前冲突文件在当前分支中修改的副本。
  - 编号为3的暂存区用于保存当前冲突文件在合并版本中修改的副本。
  - 对暂存区三个副本的访问方式：git show :编号:文件名
    - 编号：1，2，3，
    - 文件名：_config.yml

3. 工作区的版本则可能同时包含了成功合并即冲突合并，其中冲突合并会用特殊的标记（<<<<<<======>>>>>>）进行标识：
  - 特殊标识<<<<<<和======之间的内容是当前分支更改的内容。
  - 特殊标识======和>>>>>>之间的内容是所合并的版本更改的内容。

<a id=3>三. 冲突的解决</a>
冲突解决的实质就是通过编辑操作，将冲突标识符所标识的冲突内容替换为合适的内容，并去掉冲突标识符。编辑完成后执行`git add`命令将文件添加到暂存区（标号0）,然后再提交就完成了冲突解决。
1. 手工编辑完成冲突解决

   

   - 找到冲突的地方

   - 在工作区编辑文件冲突的地方

   - `git add -u`，添加到暂存区

   - `git commit -m "冲突解决"`,提交完成后，.git目录下与合并相关的文件.git/MERGE_HEAD、.git/MERGE_MSG、.git/MERGE_MODE文件都自动删除。

   - 执行推送

     

2. 图形工具完成冲突解决

   

   第一步： 在操作系统中安装图形工具：kdiff3，meld，araxis等

   第二步： 执行命令`git mergetool`后，会显示支持的图形工具列表。

   第三步：启动kdiff3后，上方窗口从左至右显示冲突文件的三个版本，分别是：

   ​                A：暂存区1中的版本（共同的先祖版本）

   ​                B：暂存区2中的版本（当前分支更改的版本）
   
   ​                C：暂存区3中的版本（他人更改的版本）![image-20210620212217851](https://gitee.com/nsaction/blog_pic/raw/master/image-20210620212217851.png)

  第四步：点击标记为“合并冲突”的一行，在弹出的菜单中出现A/B/C三个选项，分别代表从A、B、C三个窗口复制相关内容到当前位置
  第五步：B行代表从窗口B中复制的文件，M指手工修改的行
    ![image-20210620213323429](https://gitee.com/nsaction/blog_pic/raw/master/image-20210620213323429.png)

  第六步：编辑完成后保存退出即完成图形化冲突解决。

  第七步：`git status`显示工作区状态，会看到冲突已经解决，在工作区会遗留一个以.orig结尾的合并前的文件副本,暂存区中三个文件的副本也已经清除。

  第八步：执行提交和推送，由于文件是在暂存区更改，所以不需要add，直接commit，再push。

3. 树冲突可以采用手工操作和图形操作

  - 手工操作由用户决定删除文件，保留一个文件而解决冲突。`git rm 文件名`。

  - 图形模式交互式解决树冲突：执行`git mergetool`命令，忽略其中的提示和警告——>询问对冲突文件的处理方式——>输入D删除冲突文件，输入C保留冲突文件——>最终解决冲突提交。

    

---

### 总结：为了减少冲突，在我们每次在工作区编辑文件的时候，我们应该养成一个先`pull`的习惯，编辑完文件再提交，减少冲突发生。

