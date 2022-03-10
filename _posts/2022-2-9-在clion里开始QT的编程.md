---
layout: post
title: 在clion里开始QT的编程
categories: 日志
tags: 
    - 教程
    - 大二
BGImage: 'https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/20220310123346.png'
jekyll-theme-WuK:
    musicid: '34367899'
---

## 先简单阐述一下这样做的优势

- CLion本身的优秀，逐渐向VS看齐的全面的功能。也是面向未来的优秀全能开发平台。JetBrains系列用户无需重新上手
- 强大的错误提示和警告提示
- 强大的代码补全，重构，生成，分析功能
- 配合插件库实现生产效率的最大化
- 强大的调试分析能力

说完优点必须说说缺点

- 项目需要为cmake格式，qmake格式的项目需要进行简单的转化（因为代码相同，只是编译步骤不同，但是编译结果相同）
- 每次开始写项目都需要做少量工作，比如配置cmakelist文件
- 相比QTCreator，运行速度会稍慢（CLion软件占用内存更大）
- 不能同时安装msvc和mingw的QT库，这会导致一些问题。最好的解决方法是重装
- CLion收费，在此不提供任何有关的破解方案。

Tips:

- 你不需要配置外部uic工具，cmake会帮你做uic的转化（ui文件编译为c++）
- 你不需要配置.ui文件打开方式，CLion会帮你打开QTCreator来绘制窗口



### 下载安装QT

https://mirrors.tuna.tsinghua.edu.cn/qt/

### 下载安装cmake

QTCreator不自带cmake，下一个装就好





















### Qt5Config.cmake找不到

```
set(CMAKE_PREFIX_PATH "E:/Qt/Qt5.12.11/5.12.11/mingw73_64/lib/cmake/Qt5")
```

找到Qt5Config.cmake路径，添加上面这句

### ui的头文件找不到

- 假设生成的目标为`Test`，在`CMakeLists.txt`文件的最后一行添加：

```
target_include_directories(Test PRIVATE "${CMAKE_BINARY_DIR}/Test_autogen/include")#Test_autogen文件夹名字注意，Test要改成项目名！！
```

### 首次运行QT0xC0000135错误

编译成功后无法运行缺依赖

![image-20210716193316105](https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20210716193316105.png)

学官网加环境变量是最好的



解决方案是

编译完成后，在qt里找到windeployqt.exe 运行

```shell
windeployqt.exe xxxx.exe
#XXXX.exe就是你编译后的exe
```

它会把依赖文件复制到编译文件夹内

然后再运行

上述步骤只需要进行一次，除非你引用了新的库





### 编译到链接库阶段报错moc_mainwindow.cpp:99: undefined reference to ‘__imp__

```shell
CMakeFiles\QTPlayer.dir/objects.a(mainwindow.cpp.obj): In function `MainWindow::MainWindow(QWidget*)':
E:/GitProject/QTPlayer/mainwindow.cpp:7: undefined reference to `__imp__ZN10QWebSocketC1ERK7QStringN18QWebSocketProtocol7VersionEP7QObject'
E:/GitProject/QTPlayer/mainwindow.cpp:7: undefined reference to `__imp__ZN10QWebSocketD1Ev'
CMakeFiles\QTPlayer.dir/objects.a(mainwindow.cpp.obj): In function `MainWindow::connectServer()':
E:/GitProject/QTPlayer/mainwindow.cpp:16: undefined reference to `__imp__ZN10QWebSocket9connectedEv'
E:/GitProject/QTPlayer/mainwindow.cpp:18: undefined reference to `__imp__ZN10QWebSocket4openERK4QUrl'
CMakeFiles\QTPlayer.dir/objects.a(mainwindow.cpp.obj): In function `MainWindow::onConnected()':
E:/GitProject/QTPlayer/mainwindow.cpp:23: undefined reference to `__imp__ZN10QWebSocket15sendTextMessageERK7QString'
CMakeFiles\QTPlayer.dir/objects.a(mainwindow.cpp.obj): In function `MainWindow::~MainWindow()':
E:/GitProject/QTPlayer/mainwindow.cpp:36: undefined reference to `__imp__ZN10QWebSocketD1Ev'
```

![image-20210814211514401](https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20210814211514401.png)

三类问题，第一是在头文件声明的函数没写定义(网上说的)

（我不是这个问题，而且我觉得这不会导致编译直接过不了）

第二个是编译链同时装了msvc和mingw的库，混合编译了。解决方案见最后

https://stackoverflow.com/questions/39063110/lots-of-undefined-reference-in-qt-creator

第三种问题和用了websocket等插件类（它官网是这么描述的）的类功能，需要进行如下修改

```cmake
find_package(QT NAMES Qt6 Qt5 COMPONENTS WebSockets Widgets REQUIRED)


target_link_libraries(QTPlayer PRIVATE Qt5::Widgets Qt5::WebSockets)
```

这类似于qt的pro文件里的

```
QT+=WebSockets
```

新发现一种情况，在qtcreator可以编译clion却不行，原因是多套qt，没有用正确的qt

解决方案是

```
-DCMAKE_BUILD_TYPE:STRING=Debug
-DCMAKE_C_COMPILER:STRING=D:/Qt/Qt5.12.11/Tools/mingw730_64/bin/gcc.exe
-DCMAKE_GNUtoMS:BOOL=OFF
-DCMAKE_INSTALL_PREFIX:PATH=C:/Program
Files
(x86)/XiaoZhi
-DCMAKE_PREFIX_PATH:STRING=D:/Qt/Qt5.12.11/5.12.11/mingw73_64
-DCMAKE_PROJECT_INCLUDE_BEFORE:PATH=D:/Qt/Qt5.12.11/Tools/QtCreator/share/qtcreator/package-manager/auto-setup.cmake
-DQT_DIR:PATH=D:/Qt/Qt5.12.11/5.12.11/mingw73_64/lib/cmake/Qt5
-DQT_QMAKE_EXECUTABLE:STRING=D:/Qt/Qt5.12.11/5.12.11/mingw73_64/bin/qmake.exe
-DQt5Core_DIR:PATH=D:/Qt/Qt5.12.11/5.12.11/mingw73_64/lib/cmake/Qt5Core
-DQt5Gui_DIR:PATH=D:/Qt/Qt5.12.11/5.12.11/mingw73_64/lib/cmake/Qt5Gui
-DQt5Widgets_DIR:PATH=D:/Qt/Qt5.12.11/5.12.11/mingw73_64/lib/cmake/Qt5Widgets
-DQt5_DIR:PATH=D:/Qt/Qt5.12.11/5.12.11/mingw73_64/lib/cmake/Qt5
```

复制于qtcreator

![image-20220308213947672](https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20220308213947672.png)

