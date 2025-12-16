---
layout: post
title: xtx第15周日常开发日记
categories: 日志
tags: 
    - 日志 
    - 2020日志
---

# xtx第15周日常开发日记 

## 12.18

记一下java的vm启动参数

```
-Dfile.encoding=UTF-8
-Xmx3G
-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5010
```

硬核参数

```
-Dfile.encoding=UTF-8
-d64
-XX:+AggressiveOpts
-XX:+UseConcMarkSweepGC
-XX:+UseParNewGC
-XX:+CMSConcurrentMTEnabled
-XX:ParallelGCThreads=8
-Dsun.rmi.dgc.server.gcInterval=3600000
-XX:+UnlockExperimentalVMOptions
-XX:+ExplicitGCInvokesConcurrent
-XX:MaxGCPauseMillis=50
-XX:+AlwaysPreTouch
-XX:+UseStringDeduplication
-Dfml.ignorePatchDiscrepancies=true
-Dfml.ignoreInvalidMinecraftCertificates=true
-XX:-OmitStackTraceInFastThrow
-XX:+OptimizeStringConcat
-XX:+UseAdaptiveGCBoundary
-XX:NewRatio=3
-Dfml.readTimeout=90
-XX:+UseFastAccessorMethods
```

当然idea也要改文件编码

## 12.19

**不要把.idea 文件发到github，否则协作者会出问题**

如果传了要手动去网上删或者先在本地删了然后提交到github



instanceof 是 Java 的一个二元操作符，类似于 ==，>，< 等操作符。

instanceof 是 Java 的保留关键字。它的作用是测试它左边的对象是否是它右边的类的实例,返回Boolen

如

```java
Object testObject = new ArrayList();
if(testobject instanceof ArrayList) //true
```



[runoob.com/java/method-instanceof.html]: runoob.com/java/method-instanceof.html



## 12.20

查看包路径的方法

![image-20201220004035393](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20201220004035393.png)





在调试器里的f标记的变量是final的意思

要在调试的变量调试器里看看是个什么类型变量，在右键的

![](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20201220014343222.png)

改完就可以了

![image-20201220014441913](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20201220014441913.png)

也第一次验证了重新加载类变更的功能

在输出字符串有点用，引用的字符串也可以

但是注意重载需要时间，而且在调试选项里



## 2021.1.28

```commonlisp
c:/progra~2/gnuarm~1/102020~1/bin/../lib/gcc/arm-none-eabi/10.2.1/../../../../arm-none-eabi/bin/ld.exe: CMakeFiles/LED.elf.dir/Core/Src/sysmem.c.obj: in function `_sbrk':
D:\stm\LED\Core\Src/sysmem.c:55: multiple definition of `_sbrk'; CMakeFiles/LED.elf.dir/Core/Src/syscalls.c.obj:D:\stm\LED\Core\Src/syscalls.c:118: first defined here
c:/progra~2/gnuarm~1/102020~1/bin/../lib/gcc/arm-none-eabi/10.2.1/../../../../arm-none-eabi/bin/ld.exe: CMakeFiles/LED.elf.dir/startup/startup_stm32f103xb.s.obj:(.isr_vector+0x0): multiple definition of `g_pfnVectors'; CMakeFiles/LED.elf.dir/Core/Startup/startup_stm32f103c8tx.s.obj:(.isr_vector+0x0): first defined here
c:/progra~2/gnuarm~1/102020~1/bin/../lib/gcc/arm-none-eabi/10.2.1/../../../../arm-none-eabi/bin/ld.exe: CMakeFiles/LED.elf.dir/startup/startup_stm32f103xb.s.obj: in function `Default_Handler':
D:\stm\LED\startup/startup_stm32f103xb.s:112: multiple definition of `Default_Handler'; CMakeFiles/LED.elf.dir/Core/Startup/startup_stm32f103c8tx.s.obj:D:\stm\LED\Core\Startup/startup_stm32f103c8tx.s:112: first defined here
Memory region         Used Size  Region Size  %age Used
             RAM:        2656 B        20 KB     12.97%
           FLASH:        5068 B        64 KB      7.73%
collect2.exe: error: ld returned 1 exit status
mingw32-make.exe[3]: *** [CMakeFiles\LED.elf.dir\build.make:402: LED.elf] Error 1
mingw32-make.exe[2]: *** [CMakeFiles\Makefile2:95: CMakeFiles/LED.elf.dir/all] Error 2
mingw32-make.exe[1]: *** [CMakeFiles\Makefile2:102: CMakeFiles/LED.elf.dir/rule] Error 2
mingw32-make.exe: *** [Makefile:137: LED.elf] Error 2
```

```
BGImage:'https://cdn.jsdelivr.net/gh/xutongxin1/xutongxin1.github.io@bebc52fb1b67a08f8db0026051b9716a88a37900/asset/%E6%97%A5%E5%BF%97/75065066_p0.jpg'
jekyll-theme-WuK:
    musicid: '439138110'
```

