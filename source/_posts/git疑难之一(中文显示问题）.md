---
title: git疑难之一(中文显示问题）
categories:
  - git
  - 疑难
tags:
  - git
  - 中文显示
  - log
date: 2021-06-23 19:43:11
update: 2021-06-23 19:43:11
---



解决git中文显示问题

<!-- more -->





1. git输出文件名时，文件名的中文不能正确显示，而是显示为八进制的字符编码，如下：

   ~~~
   git status -s 
   ?? "\350\257\264\346\230\216.txt" $ printf "\350\257\264\346\230\216.txt\n"
   ~~~

 <font size=5 color=red>解决办法：</font>

将git配置变量core.quotepath设置为false，就可以解决文件名中的中文在这些git输出中的显示问题



~~~~
git config --global core.quotepath false  
git config --global core.quotepath off       #用这个也可以
~~~~



<font color=blue>**扩展**：</font>

- 使用以下命令，提交命令的时候使用utf-8编码集提交，这样在使用`git commit`，commit对象中嵌入正确的编码说明。

  

  ~~~
  $ git config --global i18n.commitEncoding utf-8
  ~~~




- 使用以下命令，将提交说明所使用的字符集设置为utf-8，这样使用`git log`查看提交说明时才能够正确显示其中的中文

  ~~~
  $ git config --global i18n.logOutputEncoding utf-8
  ~~~

