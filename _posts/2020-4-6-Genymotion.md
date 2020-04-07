---
layout: post
title: Genymotion安装与使用
categories: 软件教程
tag: 软件 教程 Genymotion
---



# Genymotion安装与使用
## 0：为何使用Genymotion
`Genymotion优势众多，包括默认X86的架构使其可以在电脑上最快速的运行，openGL的加速`

以上都不是我最看重的，它是最轻量级的安卓模拟器（因为系统是靠Android SDK，其余啥都没有）
而且是一台**完整的**手机，自带root，可以同时运行应用，应用可以多开。你能想到的手机功能它都有了

最最重要的是，和一般的游戏安卓模拟器相比，最高支持安卓9.0，这让某些高要求游戏在电脑上游玩挂机成为可能

~~写了几次都还是不适应写介绍语~~

## 1：下载Genymotion
直接进 [https://www.genymotion.com/download/](https://www.genymotion.com/download/)

注册一个账号（待会用到，国内邮箱都可以），然后就可以下载了
### 1.1：Win
如果你不知道什么是Virtualbox，直接点包含就好

![win](https://xutongxin1.github.io/picture/genymotion/G-1.jpg)

下载完之后安装，一路Next，请不要选择中文路径安装，**也不要使用中文用户名安装，切记**

安装完成后桌面就会有Genymotion图标了，直接运行即可

### 1.2 linux
首先apt安装Virtualbox
```
sudo apt update
sudo apt install virtualbox
```

下载好Genymotion，比如

![linux](https://github.com/xutongxin1/xutongxin1.github.io/blob/master/picture/genymotion/G-1.1.PNG)
```
wget https://dl.genymotion.com/releases/genymotion-3.0.4/genymotion-3.0.4-linux_x64.bin
```
别想着这一步就好了，还是要去官网老实注册账号的

```
sudo bash genymotion-3.0.4-linux_x64.bin
```
中间按一次会询问一次你的安装路径，我选择默认按Y

![](https://github.com/xutongxin1/xutongxin1.github.io/blob/master/picture/genymotion/G-2.PNG)

启动的话
```
cd /opt/genymobile/genymotion(默认路径)
./genymotion
```
就启动了

## 2.创建第一台虚拟机
（以win界面为例）

界面为全英文界面

![](https://github.com/xutongxin1/xutongxin1.github.io/blob/master/picture/G-3.PNG)

在下面选择一台设备类型便可以安装了

![](https://github.com/xutongxin1/xutongxin1.github.io/blob/master/picture/G-4.PNG)

最需要改的是Processor(s)核心数量（理论可以超线程分配但不建议），Memory size内存大小（这个就别超了，蓝屏等着你），如果你还想同时开别的东西就别分满

Network mode默认就好，有需要改的建议自己了解一下 [虚拟机网络类型](https://www.cnblogs.com/ct20150811/p/5143711.html)

其余选项看我的视频讲解，最后点安装就可以了。日后所有选项（除了除了Android版本不能改）

## 3.开机并安装（理论上通常使用所）必须的东西
右键创建好的虚拟机开机就好，其余右键选项见视频

开机报错可能见后文

右边栏包括了：虚拟传感器和手机状态

![](https://xutongxin1.github.io/picture/G-6.1.PNG)

虚拟的物理按键（总之按照一台手机用就好）

![](https://github.com/xutongxin1/xutongxin1.github.io/blob/master/picture/G-6.2.PNG)

### 3.1安装apk
直接把apk拖入虚拟机框框内就会自动安装。

如果你遇到下图问题，往下看

### 3.2安装ARM_Translation
由于某些应用以arm架构打包而不是X86，因此需要往虚拟机安装此包来解决安装包的架构问题。

下载地址 [https://github.com/m9rco/Genymotion_ARM_Translation/tree/master/package]()没9.0我也不知道

直接把下载好的包**（不用解压）放在纯英文路径**，然后拖入虚拟机中就可以了

### 3.3安装谷歌框架
你可能需要谷歌套件让你的应用工作，但虚拟机不自带谷歌环境(版权问题)

直接单击右上角按钮即可安装

![](https://github.com/xutongxin1/xutongxin1.github.io/blob/master/picture/G-7.PNG)

## 4进阶内容问答
### 4.0开机失败
看看有没有CPU有关字眼，如果有就可能与VT-X和AMD-X没开有关，还有可能与电脑上VM服务冲突有关

建议自己百度或者谷歌一下
### 4.1下载虚拟机环境太慢
别担心，设置中可以设置下载代理（不是虚拟机代理）
### 4.2安装谷歌app时仍然出现openGapp安装失败
我遇到了相同的问题，目前认为是安装软件的用户是中文名（或曾经是）的问题，最佳选项是卸载了重新用英文用户名安装（待确认）
### 4.3我装了谷歌套件后想要启动代理上网，但是一启动就没响应了
推测是虚拟机本身和主机有adb的连接，强行全局代理让adb断开（这种情况只能强关虚拟机再启动）

目前有两种解决方案：

1.以某个软件为例，如图

![](https://github.com/xutongxin1/xutongxin1.github.io/blob/master/picture/G-8.PNG)

![](https://github.com/xutongxin1/xutongxin1.github.io/blob/master/picture/G-9.PNG)

2.设置wifi使用外部代理

长按wifi或找到齿轮，找到高级设置

![](https://github.com/xutongxin1/xutongxin1.github.io/blob/master/picture/G-10.1.PNG)

设置代理

![](https://github.com/xutongxin1/xutongxin1.github.io/blob/master/picture/G-10.2.PNG)

这个方案没有证实，但是里面的Webview可以上Google了

### 4.4为什么没有自带浏览器
其实有，叫Webview，但是用途小，似乎不能下载文件，有需要还是自己装个chrome
### 4.5我始终下不了虚拟机环境
我这里分两个自己装好谷歌环境的镜像，一个Android8.0，一个Android9.0
### 4.6我想在服务器装着挂机可以吗？
理论上可以。

但你必须购买裸金属服务器，普通服务器是虚拟cpu出来的，不能支持VT—X，裸金属服务器是直接给你一块cpu的可以满足条件，但一般价格不低

### 4.7我想把虚拟机分享给别人可以吗？
可以，关闭genymotion，直接打开virtualbox
（对linux，直接终端输入virtualbox）

选项选择export就可以导出了，导入也直接import。

但是我测试了一下，似乎不能直接在Virtualbox启动虚拟机，还是要回到Genymotion

