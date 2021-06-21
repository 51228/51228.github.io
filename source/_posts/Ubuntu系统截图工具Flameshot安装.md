---
title: Ubuntu系统截图工具Flameshot安装
categories:
  - 小工具
tags:
  - flameshot
  - ubuntu
  - 截图
date: 2021-06-21 21:43:24
update: 2021-06-21 21:43:24
---




> #### 习惯了用微信的截图工具，在Ubuntu系统下的截图工具怎么都不习惯，不是功能太过简单，就是没有文字标注。网上介绍的Shutter功能不足，GIMP太过复杂，Ubuntu自身带的截图工具功能也太过简陋。终于找到了flameshot，但flameshot的普通安装（`sudo apt install flameshot`）没有标注文字等一些功能。本文介绍flameshot全部功能的安装。
>
> <!-- more -->



本文参考链接：https://blog.csdn.net/xiaoqiangclub/article/details/105516383



先上两张图：

![image-20210621215841178](https://gitee.com/nsaction/blog_pic/raw/master/image-20210621215841178.png)





![image-20210621215934165](https://gitee.com/nsaction/blog_pic/raw/master/image-20210621215934165.png)



#### 以上2张图可以看出全功能版本的flameshot的功能比较丰富：enter可以全屏截图，鼠标右键可以选择画笔的颜色，鼠标滚轮可以调节画笔的粗细，空格键可以调出工具栏。文字标注，添加数字递增圆圈等功能还是比较出彩的。



## 安装步骤：



#### 一、安装git，为后面克隆flameshot做准备



~~~  git
sudo apt install git 
~~~



#### 二、克隆项目到本地，我克隆到/home/sky/SoftWare/目录下。



~~~
sudo git clone https://github.com/lupoDharkael/flameshot.git
~~~



#### 三、接下来是根据 `github官方文档` 的提示安装一些依赖库



~~~
# Compile-time
apt install g++ build-essential qt5-default qt5-qmake qttools5-dev-tools

# Run-time
apt install libqt5dbus5 libqt5network5 libqt5core5a libqt5widgets5 libqt5gui5 libqt5svg5-dev

# Optional
apt install git openssl ca-certificates

~~~



![image-20210621222006062](https://gitee.com/nsaction/blog_pic/raw/master/image-20210621222006062.png)



#### 四、在克隆的目录（/home/sky/SoftWare/flameshot/）下进行编译：



~~~
mkdir build
cd build
cmake ../
make
~~~

在这步有如果你用`sudo apt install cmake`，可能会遇到cmake版本过低的问题，在https://cmake.org/files/网站下载最新版本安装即可。



#### 五、编译完成之后执行命令`sudo make install`，安装完成！

#### 六、使用命令`which flameshot`可以得到flameshot的位置

#### 七、在设置中添加自定义快捷键

![image-20210621223410545](https://gitee.com/nsaction/blog_pic/raw/master/image-20210621223410545.png)



#### 八、现在你可以通过快捷键或者通过命令来启动flameshot，当然命令可以加入环境变量。



### 到此，你就有一个完整版的flameshot使用了。
