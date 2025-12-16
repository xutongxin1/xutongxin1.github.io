# Clion+STM32配置环境

## 下载安装

下载Clion，具体步骤省略
https://www.jetbrains.com/clion/download/

Clion官方的教程网址如下

https://www.jetbrains.com/help/clion/embedded-development.html

需要下载的东西有openocd和GNU ARM工具链

openocd：https://gnutoolchains.com/arm-eabi/openocd/（最新版即可）

![image-20230713195950638](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202307132010898.png)

GNU ARM工具链：https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads（最新版即可）

![image-20230713201216119](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202307132012205.png)

OpenOCD强烈建议安装在无空格，无特殊符号，尽可能简短的路径

安装后需要配置环境变量，版本号可能不一样（更别说yourpath了）别照抄

```
C:\yourpath\OpenOCD-20230202-0.12.0\bin
C:\yourpath\Arm GNU Toolchain arm-none-eabi\12.2 rel1\bin
```

假设你没有用过CubeMX，下载地址与推荐学习帖子：

https://www.st.com/en/development-tools/stm32cubemx.html#get-software

http://www.openedv.com/thread-309468-1-1.html

## 创建项目

**项目路径和名字不要有空格，特殊符号，最好下划线也不要！！**

先使用stm32Cubemx创建项目，在创建代码时需要选择STM32CubeIDE选项

![image-20230521215546852](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230521215546852.png)

已经创建了项目的ico？(且要求ide没有选错)

直接在Clion内打开项目，选择打开ico文件，会自动识别为一个项目

板载文件.cfg配置，他给你推荐，你搜索对应的型号，大概就行（里面全部是开发板的cfg，然而我们大概率不是在用列表里的开发板），然后复制到项目中使用

![image-20230522182207543](https://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img3/image-20230522182207543.png)

![image-20230713203548566](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202307132035725.png)

OpenOCD在Clion的配置的话大概如图（大概只需要改一次）

![image-20230713201503905](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202307132015979.png)

### 稍微提一下cmake语法问题

![image-20230713202639850](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202307132026921.png)

这句话意思是索引在Startup，Src，Drivers下的所有文件夹里的所有文件，所以如果你添加了文件，显示找不到，请手动更新cmake

![image-20230713202751582](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202307132027639.png)

cmake存在缓存，如果还是没刷出来可以考虑清缓存

![image-20230713202829031](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202307132028143.png)

运行/调试配置要修改

![image-20230713203710250](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202307132037366.png)

调试和运行的对象是这个图标的，别搞错了![image-20230713203754169](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202307132037228.png)

## 其他

### keil内的编译选项，宏定义在哪里添加

![image-20230713203000117](http://github.xutongxin.me/https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/202307132030187.png)

在cmake中添加这句话，即定义DEBUG，USE_STDPERIPH_DRIVER等三个宏定义，类似的还有-O0等

### printf怎么用

添加文件，记得自己改h7xx

retarget.h

```c
#include "stm32H7xx_hal.h"
#include <sys/stat.h>
#include <stdio.h>

void RetargetInit(UART_HandleTypeDef *huart);

int _isatty(int fd);

int _write(int fd, char *ptr, int len);

int _close(int fd);

int _lseek(int fd, int ptr, int dir);

int _read(int fd, char *ptr, int len);

int _fstat(int fd, struct stat *st);

```

retarget.c

```c
#include <_ansi.h>
#include <_syslist.h>
#include <errno.h>
#include <sys/time.h>
#include <sys/times.h>
#include <stdint.h>

#if !defined(OS_USE_SEMIHOSTING)

#define STDIN_FILENO  0
#define STDOUT_FILENO 1
#define STDERR_FILENO 2

UART_HandleTypeDef *gHuart;

void RetargetInit(UART_HandleTypeDef *huart)
{
    gHuart = huart;
    
    /* Disable I/O buffering for STDOUT stream, so that
     * chars are sent out as soon as they are printed. */
    setvbuf(stdout, NULL, _IONBF, 0);
}

int _isatty(int fd)
{
    if (fd >= STDIN_FILENO && fd <= STDERR_FILENO)
        return 1;
    
    errno = EBADF;
    return 0;
}

int _write(int fd, char *ptr, int len)
{
    HAL_StatusTypeDef hstatus;
    
    if (fd == STDOUT_FILENO || fd == STDERR_FILENO)
    {
        hstatus = HAL_UART_Transmit(gHuart, (uint8_t *) ptr, len, HAL_MAX_DELAY);
        if (hstatus == HAL_OK)
            return len;
        else
            return EIO;
    }
    errno = EBADF;
    return -1;
}

int _close(int fd)
{
    if (fd >= STDIN_FILENO && fd <= STDERR_FILENO)
        return 0;
    
    errno = EBADF;
    return -1;
}

int _lseek(int fd, int ptr, int dir)
{
    (void) fd;
    (void) ptr;
    (void) dir;
    
    errno = EBADF;
    return -1;
}

int _read(int fd, char *ptr, int len)
{
    HAL_StatusTypeDef hstatus;
    
    if (fd == STDIN_FILENO)
    {
        hstatus = HAL_UART_Receive(gHuart, (uint8_t *) ptr, 1, HAL_MAX_DELAY);
        if (hstatus == HAL_OK)
            return 1;
        else
            return EIO;
    }
    errno = EBADF;
    return -1;
}

int _fstat(int fd, struct stat *st)
{
    if (fd >= STDIN_FILENO && fd <= STDERR_FILENO)
    {
        st->st_mode = S_IFCHR;
        return 0;
    }
    
    errno = EBADF;
    return 0;
}

#endif //#if !defined(OS_USE_SEMIHOSTING)
```

main.c内

```c
    RetargetInit(&huart1);
```

编译后提示重复函数名，需要自己屏蔽syscall的同名函数

对于浮点数打印，还有加一句cmake语句

```
set(COMMON_FLAGS "-specs=nosys.specs -specs=nano.specs -u _printf_float ")
```

