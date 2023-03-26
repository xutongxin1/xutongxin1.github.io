# xtx的八股文总结

## ARM底层

#### 中断函数的要求

```
1、中断函数不能进行参数传递

2、中断函数没有返回值

3、在任何情况下都不能直接调用中断函数

4、中断函数使用浮点运算要保存浮点寄存器的状态。

5、如果在中断函数中调用了其它函数，则被调用函数所使用的寄存器必须与中断函数相同，被调函数最好设置为可重入的。

6、（可忽略）C51编译器对中断函数编译时会自动在程序开始和结束处加上相应的内容，具体如下：

在程序开始处对ACC、B、DPH、DPL和PSW入栈，结束时出栈。

中断函数未加using n修饰符的，开始时还要将R0~R1入栈，结束时出栈。

如中断函数加using n修饰符，则在开始将PSW入栈后还要修改PSW中的工作寄存器组选择位。

C51编译器从绝对地址8m 3处产生一个中断向量，其中m为中断号，也即interrupt后面的数字。该向量包含一个到中断函数入口地址的绝对跳转。

7、中断函数最好写在文件的尾部，并且禁止使用extern存储类型说明。防止其它程序调用。

8、在设计中断时，要注意的是哪些功能应该放在中断程序中，哪些功能应该放在主程序中。一般来说中断服务程序应该做最少量的工作，这样做有很多好处。

首先系统对中断的反应面更宽了，有些系统如果丢失中断或对中断反应太慢将产生十分严重的后果，这时有充足的时间等待中断是十分重要的。

其次它可使中断服务程序的结构简单，不容易出错。中断程序中放入的东西越多，他们之间越容易起冲突。简化中断服务程序意味着软件中将有更多的代码段，但可把这些都放入主程序中。

9、中断服务程序的设计对系统的成败有至关重要的作用，要仔细考虑各中断之间的关系和每个中断执行的时间，特别要注意那些对同一个数据进行操作的中断
```

#### 软中断和硬中断的区别 硬中断怎么产生 

https://zhuanlan.zhihu.com/p/85597791

软中断是执行中断指令产生的，而硬中断是由外设引发的

硬中断是可屏蔽的，软中断不可屏蔽。

硬中断处理程序要确保它能快速地完成任务，这样程序执行时才不会等待较长时间，称为上半部。

软中断处理硬中断未完成的工作，是一种推后执行的机制，属于下半部。



由与系统相连的外设(比如网卡、硬盘)自动产生的。主要是用来通知操作系统系统外设状态的变化。比如当网卡收到数据包的时候，就会发出一个中断。我们通常所说的中断指的是硬中断(hardirq)。

#### 中断的响应的整个过程

![在这里插入图片描述](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/20210607194058569.png)



## stm32硬件，通信协议有关

#### STM32的GPIO电平状态

 高低电平 高阻态

#### GPIO的8个状态的各个特点

https://blog.csdn.net/welbell_uplooking/article/details/90904080（建议和下面交叉食用）

https://zhuanlan.zhihu.com/p/169587595（讲推挽和开漏可以）

一、输入状态：浮空输入，上拉输入，下拉输入，模拟输入（AD检测用）

二、输出状态：推挽输出，开漏输出，复用推挽输出，复用开漏输出

![image-20220616120340449](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20220616120340449.png)

![image-20220616120647163](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20220616120647163.png)

一般来说，开漏是用来连接不同电平的器件，匹配电平用的，因为开漏引脚不连接外部的上拉电阻时，只能输出低电平，如果需要同时具备输出高电平的功能，则需要接上拉电阻，很好的一个优点是通过改变上拉电源的电压，便可以改变传输电平。比如加上上拉电阻就可以提供TTL/CMOS电平输出等。（上拉电阻的阻值决定了逻辑电平转换的沿的速度。阻值越大，速度越低功耗越小，所以负载电阻的选择要兼顾功耗和速度。）

#### 通讯协议 iic特征 iic时序图 

![image-20220616131419029](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20220616131419029.png)

#### 单工 半双工 全双工是什么 

https://www.eet-china.com/mp/a69082.html

![image-20220616121028418](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20220616121028418.png)

#### 串口通讯传输过程 spi最少几根线 SPI通信时序

![image-20220616121112212](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20220616121112212.png)

MOSI MISO CLK（别忘了时钟线）

https://blog.csdn.net/zhiyuan2021/article/details/76629796

时序图与CPOL和CPHA有关

![img](https://raw.githubusercontents.com/xutongxin1/PictureBed/master/img0/28e6ddff556fd10ea8ad4b4357f0618b.png)

#### flash是什么 flash的读写有什么特点

目前Flash主要有两种NORFlash和NANDFlash。

单片机里的一般是NorFlash，写入慢，读取快，控制方便

NANDFlash比如SD卡，写入快，读取稍慢，控制复杂（需要专门的控制芯片）

## C/C++语言有关

### 重载、重写和重定义的区别

https://blog.csdn.net/Liuqz2009/article/details/107280075

重写：参数一样，父类函数要有virtual

重载：参数可以不一样，多个共存

### new与malloc

https://www.cnblogs.com/qg-whz/p/5140930.html

![image-20220705014022188](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20220705014022188.png)



## Linux直接相关

### 操作系统的启动过程



**裸机Linux系统的引导：** 
一个SOC(片上系统芯片)拿过来，它是有内部BROM和SRAM的，这个BROM中会固化芯片厂商的最初引导代码，我们叫它RBL(ROM boot loader)，它是SOC上电后开始运行的地方，它会判断是哪种启动方式，如果是nand启动，就会从nand的起始地址处读取UBL（user boot loader）并且复制到[ARM](https://so.csdn.net/so/search?q=ARM&spm=1001.2101.3001.7020)的内存里面，也就是上面说的片内SRAM，UBL运行在ARM的内存里,初始化系统,例如初始化DDR.然后UBL从NAND Flash里面读取U-Boot的内容并且复制到DDR里运行.DDR里面运行的U-Boot又从NAND Flash里面读取Linux内核代码,并且复制到DDR上,然后启动内核. 

Linux启动流程：

https://www.runoob.com/linux/linux-system-boot.html

![image-20220705013906591](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20220705013906591.png)

### 进程创建的几种方式

fork和exec

posix_spawn=fork+exec

vfork 子进程，不开辟地址空间

rfork/clone 类似进程，加强内存复制控制

```
总之，通过这些参数，clone实际上创建出了一个从属于原进程、与原进程共享大量数据结构、拥有私有栈的实例，而这个实例就对应于一个线程。
```

https://blog.csdn.net/weixin_45834777/article/details/117871767

fork有CopyOnWrite，clone相比fork的好处？
fork进程时，并不是所有的页面都需要复制，父进程的代码段和只读数据段都不被允许修改，所以无需复制。clone也是共享堆、代码段、数据段等内存，同样需要复制一份，为什么clone更优？

COW需要触发缺页异常，会陷入内核，这样会产生上下文切换的开销，同时，操作系统复制不共享的内存后需要建立内存映射，效率低下。



## 网络层等上层协议相关

3.	
4.	操作系统的启动过程（bootloader ->启动内核->根文件目录）
5.	内核的启动流程
6.	const关键字 指针常量 常量指针
7.	static修饰的变量 与 全局（局部）变量的区别 
8.	内联函数（知道但没用过）问了 优缺点 以及为什么内联函数不能过于冗杂
9.	指针的字节大小 （32 64 16位）
10.	指针和数组的区别（存储位置）
11.	
    #include <stdio.h>

#include <stdlib.h>

void getmemory(char *p)

{

    p=(char *) malloc(100);
    
    strcpy(p,"hello world");

}

int main( )

{

    char *str=NULL;
    
    getmemory(str);
    
    printf("%s/n",str);
    
    free(str);
    
    return 0;

}
问：该程序的执行结果_________
程序崩溃，getmemory中的malloc 不能返回动态内存， free（）对str操作很危险。
（知道结果 但是解释不清 刚看的面经）
17.	C++新特性 智能指针（不会） lambda表达式 问了 是什么 语法 形式
18.	链表：数组跟链表的区别 优缺点 以及什么情况用哪个
19.	linux命令行 
创建文件夹 创建多级文件夹（递归）
通过文件名查找文件 通过当前目录里文件内容查找文件
20.	编程题
字符串转换为整型数据
string a = 123123->int res =123123
考虑符号 数据有效性 数据溢出 
21.	什么是进程和线程 
22.	创建子进程fork getpid（）父进程进程id
23.	子进程和父进程 通信最快方式 mmap 别的方式
24.	父子进程如果用管道通信 用的是什么管道 命令管道 怎么实现 （不会）
25.	多线程 线程创建函数参数有什么 
26.	用过线程池吗 没有
27.	线程与线程之间的同步 （不会）
28.	编译阶段（4个阶段） 四个阶段各自的作用
29.	网络编程 tcp udp 两者的特性
30.	三次握手 四次挥手
31.	最后问了企业对新人的培训方式



