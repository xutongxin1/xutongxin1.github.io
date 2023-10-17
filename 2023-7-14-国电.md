三路加法器

白色袋子的金属外壳的

AD835乘法器

AD831

AD8367

电源*3

AD7606 8通道16bitADC

AD637栓波器

OP07双路运放

AD623A仪表放大器

NE564FM/FSK解调

单通道减法器

OP376运放

UAF42有源滤波器

AD9850



```shell
python
import sys, os.path
sys.path.insert(0, os.path.expanduser('~/.gdb'))
import qt5printers
qt5printers.register_printers(gdb.current_objfile())
end
```



## 初始化注意事项



### 添加cmake

```cmake
set(COMMON_FLAGS "-specs=nosys.specs -specs=nano.specs -u _printf_float ")
```

```cmake
target_link_libraries(${PROJECT_NAME}.elf ${CMAKE_SOURCE_DIR}/Middlewares/ST/ARM/DSP/Lib/libarm_cortexM4lf_math.a)
```

定时器的Interal Clock对应HCLK的时钟源

#### DDS其中一个库的初始化有问题

带扫频的库需要手动初始化所有的通道，也需要手动io更新，还需要在多次更改间delay（delay的12不用变）

不带扫频的库不需要

### H7的Uart+DMA真有毒

CPU的缓存会导致数据搬运了但实际是没有数的







PSC 687

ARR 2

200145.5604hz





21MHz SPI，现在改为25



