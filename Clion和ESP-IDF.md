**首先这个教程是面对idf框架的，不是arduino或者mpython框架。且不是用pio工具来实现。**
**本教程所准备的工具链可以完成编译，烧录，调试的全流程**

## Clion安装

从官网下载，安装。

https://www.jetbrains.com/clion/download/

破解的话，先下载工具，解压到一个不会被你误删，误放的目录

比如放这里↓

![image-20230515203956620](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202305152041083.png)

然后先使用uninstall等待ok（看提示）

![image-20230515204117563](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202305152041663.png)

再使用install等待ok

![image-20230515204136555](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202305152041618.png)

然后打开clion使用激活码

![image-20230515204149678](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202305152041742.png)

如此时显示激活码错误，那大概是前面步骤有误。前面工具是有版本区别的，2022能用的工具2023不一定可以用。

## IDF安装

使用vscode，安装idf插件，然后安装时使用esp的源，安装idf v5版本，然后等待。如果中间下载时中断出错，建议直接删除用户目录下的esp-idf与.espressif文件夹然后重试。

![image-20230515204443207](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202305152044294.png)

![image-20230515204509379](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202305152045540.png)

![image-20230515204532037](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202305152045147.png)

![image-20230515204544545](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202305152045632.png)



## 关于新建项目

clion无法完成根据项目模板创建，应该先使用vscode新建项目

![image-20230516185357759](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202305161853860.png)

![image-20230516185338401](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202305161853947.png)

## Clion配置

打开项目的cmakelist文件作为项目打开，然后cmake加载时一定报错，此时要修改构建设置，设置环境变量，添加两条，第一条是idf框架位置，第二条是你打算使用哪个com口进行烧录和监视。此时再进行加载即可正常加载cmake。一般用到的只有app flash和monitor

https://www.youtube.com/watch?v=M6fa7tzZdLw

 打开设置->构建、执行、部署->CMake->环境

![image-20230612125036168](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230612125036168.png)

![image-20230612125108722](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230612125108722.png)

环境变量添加两条

a）烧录端口（ESPPORT）：设置为对应的端口号

b）IDF_PATH：设置为idf框架位置在你电脑里的对应位置

![image-20230612125426527](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230612125426527.png)



关于添加monitor监视

设置->工具->外部工具

新建一个外部工具后，程序选择cmd，实参为

```
/k "C:\Espressif\frameworks\esp-idf-v5.0\export.bat && idf.py monitor -p COM11"
```

其中```C:\Espressif\frameworks\esp-idf-v5.0\export.bat```需要更改为你电脑中export.bat所在位置，可以用utool直接搜索，```COM11```改为你用到的端口号

工作目录照抄即可

![image-20230612125927325](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230612125927325.png)



## 关于调试

首先需要借用官方的openocd support的插件，先要在设置中修改openocd的程序路径，然后新建一个openocd运行配置，修改gdb路径，elf路径，从不下载，重启选择停止，然后在用户文件夹的.gdbinit写入以下项

```shell
set auto-load local-gdbinit on
add-auto-load-safe-path /
```

允许加载项目文件夹的.gdbinit，在项目的.gdbinit中写入如下内容加载elf等，然后即可使用

```shell
set mem inaccessible-by-default off
set remotetimeout 20
mon reset halt
flushregs
set remote hardware-watchpoint-limit 2
thb app_main
r
```

如果是Jtag调试，需要修改对应的usb协议为winusb

![image-20230715164153058](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202307151641165.png)

然后，在CLion右上打开运行/调试配置，点一下，然后选择编辑配置

![image-20230612130449229](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230612130449229.png)

![image-20230612130503062](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230612130503062.png)

新建一个“OpenOCD下载并运行”的配置

![image-20230612130538757](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230612130538757.png)

目标用app就行

可执行的二进制文件是你要调试的项目对应生成的二进制文件，类似.hex文件之类的东西，一般打开一个项目会自己加载出来，一般不用填

调试器：查找对应.exe文件，用utool查找，名字应该一样，找到后复制路径下来就行

![image-20230612130328617](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230612130328617.png)

面板配置路径除了前面的框架路径不同，后面半段的路径大差不差，直接用utool查文件名然后复制路径下来就行

![image-20230612131532740](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230612131532740.png)

GDB端口：3333       Telnet端口：4444

下载选“从不”；重置选“停止”

![image-20230612131921687](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230612131921687.png)

```
目标app
项目路径\cmake-build-debug\XMB_WirelessDebuger.elf
框架工具路径\.espressif\tools\xtensa-esp32-elf\esp-2022r1-11.2.0\xtensa-esp32-elf\bin\xtensa-esp32-elf-gdb.exe
框架工具路径\.espressif\tools\openocd-esp32\v0.11.0-esp32-20221026\openocd-esp32\share\openocd\scripts\board\esp32-wrover-kit-3.3v.cfg
```

### 快速Jtag烧录

![image-20230715164516769](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202307151645850.png)

## 注意问题

### MenuConfig在哪里怎么配置比较好

搜索cmd，管理员运行

![image-20230612132259403](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230612132259403.png)

cd进入对应项目目录

![image-20230612132331049](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230612132331049.png)

![image-20230612132428188](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230612132428188.png)

输入export.bat所在路径

![image-20230612134832676](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230612134832676.png)

然后输入```idf.py menuconfig```

![image-20230612134946050](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230612134946050.png)

进入这个界面就是menuconfig的界面了



### Monitor使用技巧

如果刚刚的外部工具添加过程没有出错的话，只要插好usb，识别到端口，，然后点你刚刚添加的外部工具就行，我这里自定义的名字是“IDF Monitor”

![image-20230612132013349](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230612132013349.png)

### Clion使用小技巧

对函数变量等中键可以查找其定义，如果对定义中键就是查找用法

使用Ctrl+Alt+左/右箭头 可以快速返回刚看的函数

对变量或者函数右键重构里面的重命名是个好东西，其他自己摸索

代码菜单里可以重新格式化代码，建议熟悉它的快捷键

 

