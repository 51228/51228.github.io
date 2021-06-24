---
title: '## git reset and git checkout'
categories:
  - git
tags:
  - "git checkout"
  - "git reset"
  - 检出
  - 重置
date: 2021-06-24 17:03:35
update: 2021-06-24 17:03:35
---



> 重置的默认值是HEAD，而检出的默认值是暂存区。因此重置一般用于重置暂存区（除非使用--hard参数，否则不重置工作区），而检出命令主要是覆盖工作区（如果`<commit>`不省略，也会替换暂存区中相应的文件）。

<!-- more -->



## git reset and git checkout



重置的默认值是HEAD，而检出的默认值是暂存区。因此重置一般用于重置暂存区（除非使用--hard参数，否则不重置工作区），而检出命令主要是覆盖工作区（如果`<commit>`不省略，也会替换暂存区中相应的文件）。



### 一、git reset



1. #### git reset命令格式

   

重置命令（git reset）是Git最常用的命令之一，也是最危险最容易误用的命令。来看看git reset命令的用法。 



- <font color=brown>用法一：`git reset [-q] [<commit>] [--] <paths>... `</font>

- <font color=brown>用法二：`git reset [--soft | --mixed | --hard | --merge | --keep] [-q] [<commit>]  `</font>

  


2. #### git reset命令解析

   

上面列出了两个用法，其中`<commit>`都是可选项，可以使用引用或提交ID，如果省略`<commit>`则相当于使用了HEAD的指向作为提交ID。

上面列出的两种用法的区别在于，第一种用法在命令中包含路径`<paths>`。为了避免路径和引用（或者提交ID）同名而发生冲突，可以在`<paths>`前用两个连续的短线（减号）作为分隔。

第一种用法（包含了路径`<paths>`的用法）不会重置引用，更不会改变工作区，而是用指定提交状态（`<commit>`）下的文件（`<paths>`）替换掉暂存区中的文件。例如命令`git reset HEAD<paths>`相当于取消之前执行的`git add<paths>`命令时改变的暂存区。

第二种用法（不使用路径`<paths>`的用法）则会重置引用。根据不同的选项，可以对暂存区或工作区进行重置。参照下面的版本库模型图（图1），来看一看不同的参数对第二种重置语法的影响。



图1：

![4A8334536628F5AE791CDEEE34205B70](https://gitee.com/nsaction/blog_pic/raw/master/4A8334536628F5AE791CDEEE34205B70.jpg)



3. #### git reset 命令的三个重要参数

   

命令格式:` git reset [--soft | --mixed | --hard ] [<commit>] `。 

- 使用参数--hard，如：`git reset--hard<commit>`。会执行上图中的全部动作①、②、③，即：

  - ①替换引用的指向。引用指向新的提交ID。

  - ②替换暂存区。替换后，暂存区的内容和引用指向的目录树一致。

  - ③替换工作区。替换后，工作区的内容变得和暂存区一致，也和HEAD所指向的目录树内容相同。

- 使用参数--soft，如：`git reset--soft<commit>`。会执行上图中的操作①。即只更改引用的指向，不改变暂存区和工作区。

- 使用参数--mixed或不使用参数（默认为--mixed），如：`git reset<commit>`。会执行上图中的操作①和操作②。即更改引用的指向及重置暂存区，但是不改变工作区。

  

4. #### git reset命令。

   


  - <font color = red>命令：`git reset`仅用HEAD指向的目录树重置暂存区，工作区不会受到影响，相当于将之前用git add命令更新到暂存区的内容撤出暂存区。引用也未改变，因为引用重置到HEAD相当于没有重置。</font>

  - 命令：`git reset HEAD`同上。

  - 命令：`git reset--filename`仅将文件filename的改动撤出暂存区，暂存区中其他文件不改变。相当于对命令`git add filename`的反向操作。

  - 命令：`git reset HEAD filename`同上。

  - 命令：`git reset--soft HEAD^`工作区和暂存区不改变，但是引用向前回退一次。当对最新提交的提交说明或提交的更改不满意时，撤销最新的提交以便重新提交。

 修补提交命令`git commit--amend`，用于对最新的提交进行重新提交以修补错误的提交说明或错误的提交文件。修补提交命令实际上相当于执行了下面两条命令。（注：文件`.git/COMMIT_EDITMSG`保存了上次的提交日志。）


    $git reset --soft HEAD^
    $ git commit -e -F .git/COMMIT_EDITMSG  


- 命令：`git reset HEAD^`工作区不改变，但是暂存区会回退到上一次提交之前，引用也会回退一次。
 - 命令：`git reset--mixed HEAD^`同上。
 - 命令：`git reset--hard HEAD^`彻底撤销最近的提交。引用回退到前一次，而且工作区和暂存区都会回退到上一次提交的状态。自上一次以来的提交全部丢失。





### 二、git checkout

1. #### git checkout命令格式

   

   检出命令（git checkout）是Git最常用的命令之一，同时也是一个很危险的命令，因为这条命令会重写工作区。检出命令的用法如下： 

   

   - 用法一： git checkout [-q] [<commit>] [--] <paths>... 

   - 用法二： git checkout [<branch>] 

   - 用法三： git checkout [-m] [[-b|--orphan] <new_branch>] [<start_point>]  

   

2. #### git checkout命令解析

  

- 上面列出的第一种用法和第二种用法的区别在于，第一种用法在命令中包含路径`<paths>`。为了避免路径和引用（或者提交ID）同名而发生冲突，可以在`<paths>`前用两个连续的短线（减号）作为分隔。`<commit>`是可选项，如果省略则相当于从暂存区（index）进行检出。

- 第一种用法（包含了路径`<paths>`的用法）不会改变HEAD头指针，主要是用于指定版本的文件覆盖工作区中对应的文件。如果省略`<commit>`，则会用暂存区的文件覆盖工作区的文件，否则用指定提交中的文件覆盖暂存区和工作区中对应的文件。

- 第二种用法（不使用路径`<paths>`的用法）则会改变HEAD头指针。之所以后面的参数写作`<branch>`，是因为只有HEAD切换到一个分支才可以对提交进行跟踪，否则仍然会进入“分离头指针”的状态。在“分离头指针”状态下的提交不能被引用关联到，从而可能丢失。所以用法二最主要的作用就是切换到分支。如果省略`<branch>`则相当于对工作区进行状态检查。

- 第三种用法主要是创建和切换到新的分支（`<new_branch>`），新的分支从`<start_point>`指定的提交开始创建。新分支和我们熟悉的master分支没有什么实质的不同，都是在`refs/heads`命名空间下的引用。



  如图2所示的版本库模型图描述了`git checkout`实际完成的操作。 

  

       图2：

![78689BA0FA5DEFE0D1ED902F9F97932A](https://gitee.com/nsaction/blog_pic/raw/master/78689BA0FA5DEFE0D1ED902F9F97932A.jpg)





3. #### git checkout命令

检出命令与版本库关系图下面通过一些示例来具体看一下检出命令的不同用法。



- 命令：`git checkout branch`检出branch分支。要完成如图2中的三个步骤，更新HEAD以指向branch分支，以及用branch指向的树更新暂存区和工作区。

- 命令：`git checkout`汇总显示工作区、暂存区与HEAD的差异。

- 命令：`git checkout HEAD`同上。

- 命令：`git checkout--filename`用暂存区中filename文件来覆盖工作区中的filename文件。相当于取消自上次执行git add filename以来（如果执行过）的本地修改。这个命令很危险，因为对于本地的修改会悄无声息地覆盖，毫不留情。

- 命令：`git checkout branch--filename`维持HEAD的指向不变。用branch所指向的提交中的filename替换暂存区和工作区中相应的文件。注意会将暂存区和工作区中的filename文件直接覆盖。

- 命令：`git checkout--.`或写作`git checkout.`注意　`git checkout`命令后的参数为一个点（“.”）。这条命令最危险！会取消所有本地的修改（相对于暂存区）。相当于用暂存区的所有文件直接覆盖本地文件，不给用户任何确认的机会！

- 命令：`git rm--cached`会直接从暂存区删除文件，工作区则不做出改变。

- 当执行git checkout HEAD.或git checkout HEAD命令时，会用HEAD指向的master分支中的全部或部分文件替换暂存区和工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。





#### <font color=blue>**扩展** </font>



<font size =4 color=blue>Git提供了很多方法可以方便地访问Git库中的对象：</font>



- 采用部分的SHA1哈希值。不必把40位的哈希值写全，只采用开头的部分（4位以上），只要不与现有的其他哈希值冲突即可。

- 使用master代表分支master中最新的提交，也可以使用全称`refs/heads/master`或`heads/master`。

- 使用HEAD代表版本库中最近的一次提交。

- 符号^可以用于指代父提交。例如：`HEAD^`代表版本库中的上一次提交，即最近一次提交的父提交。

- `HEAD^^` 则代表`HEAD^`的父提交。

- 对于一个提交有多个父提交，可以在符号^后面用数字表示是第几个父提交。

例如：·`a573106^2`的含义是提交a573106的多个父提交中的第二个父提交。

- `HEAD^1`相当于`HEAD^`，含义是HEAD的多个父提交中的第一个父提交。

- `HEAD^^2`的含义是`HEAD^`(HEAD父提交)的多个父提交中的第二个父提交。

- 符号~也可以用于指代祖先提交。例如：`a573106~5`即相当于`a573106^^^^^`。

- 提交所对应的树对象，可以用类似如下的语法访问：`a573106^{tree}`

- 某一次提交对应的文件对象，可以用如下的语法访问：`a573106:path/to/file`

- 暂存区中的文件对象，可以用如下的语法访问：`:path/to/file`



<font size =4 color=blue>git reflog命令的输出中还提供了一个方便易记的表达式：</font>



~~~
<refname>@{<n>}
~~~



这个表达式的含义是应用<refname>之前第<n>次改变时的SHAI哈希值



~~~
git reset --hard master@{2}   #重置master为两次改变之前的值
~~~







## 总结：

重置命令的一个用途就是修改引用（如master）的游标指向。实际上在执行重置命令的时候没有使用任何参数对所要重置的分支名（如master）进行设置，这是因为重置命令实际上所针对的是头指针HEAD。重置的默认值是HEAD，而检出的默认值是暂存区。因此重置一般用于重置暂存区（除非使用--hard参数，否则不重置工作区），而检出命令主要是覆盖工作区（如果`<commit>`不省略，也会替换暂存区中相应的文件）。