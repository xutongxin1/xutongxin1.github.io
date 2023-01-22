## 文件系统

1. usr指的不是用户，而是unix software recourse，里面也包含bin，lib等文件夹，一般用户的程序安装后就放置于此
2. dev下面放的是各种设备的文件
3. etc下面放各种配置文件
   1. 如有个passwd放的是用户组的权限等文件

4. lib库文件
5. boot开启启动程序
6. bin二进制可执行文件

### 文件类型

使用ls -l 可以查看到文件的具体属性，类型，权限大小等，似乎等同于ll

- -普通文件
- d文件夹
- c字符设备文件
- b块设备文件
- l软连接文件
- p管道文件
- s套接字文件

块设备：硬盘，内存

字符设备：鼠标键盘

### 文件操作的几个指令

- -r通常指的是递归进入
  - 有时候-R和-r不同，如ls -R才是递归查看
  - rm -f 指的是忽略错误
- cp如果不-r，其无法复制带文件的文件夹，会提示忽略跳过
  - cp -a也可以实现文件内的内容一起复制
  - 区别在于-a会把文件修改时间等信息一起复制过去
- cp同时可以对复制的文件重命名，即另存为
  - mv即真：重命名
- cat只能看文件全部，无参数问题，做不到只看几行
  - tac可以反过来看全部
  - cat相当于打开了文件，会修改文件信息
- more和less用于接续性一页一页看文件，空格一页回车一行，less不显示百分比，more显示，用q或者ctrl+c退出
- head无参数用于看文件前十行，有参数 -5 看五行
  - tail看倒十行
- mkdir创建文件夹，rmdir删除 **空** 文件夹，因此很鸡肋
- tree树状看目录
- cd -可以回到跳转前，然后也可以继续-回来
- ln -s用于创建软连接
  - 同一个文件的软连接的**大小**与软连接的连接位置长度有关
  - 如果要复制一个软连接去其他地方，创建软连接时应使用绝对路径
  - 软连接权限与其指向的文件的权限无关
- ln 不接参数就是硬链接
  - 对一个文件进行修改，可以改变另一个文件
  - 本质上是通过文件Inode实现（文件指针），操作系统给每一个文件赋予一个唯一的Inode，相同Inode的文件彼此同步
  - ls -l的权限后面的数字就是硬链接的数量
  - 删除硬链接时，只是硬链接数量-1，除非减到0就是真的把文件从磁盘上删除了

### 文件属性和用户组

- whoami（
- chmod
  - ugo先user后group最后other，rwx是读写执行
  - chmod u+x 就是给user+执行权限
  - a是全部人，如chmod a+x
  - 数字表示的话就是4读2写1执行
- adduser
  - 创建用户时会同时新增用户组，再在该用户组下新增一个用户
- chown
  - 需要sudo
  - 对于文件，只要用户或者用户组有权限即可做出对应操作
  - 可以使用chown xuser:xgroup file 实现一次性修改所属用户和用户组的权限
  - nobody和nogroup为系统自带
- addgroup与delgroup
- chgrp

### 继续文件操作

- file指令获取文件类型信息
- stat更详细的文件信息
- ls
  - -d只看目录
  - -h以人类的方式（）文件大小被转化为KMG

- find
  - find ./ -type 'l'在./目录下递归找软连接
  - find ./ -name '*.jpg'在./目录下递归找文件名为.jpg的文件
  - -maxdepth  2 最大搜索深度2
  - -size +20M 大于20M
    - M，G是大写，但k要小写
    - -size +20M -size -50M 20m~50m之间，记得都要加-size
  - -atime，-mtime，-ctime 最近访问时间，最近更改（状态，权限，位置）时间，最近修改（文件内容）时间
    - -amin,-mmin,-cmin
    - 意义为距离上次 n天，如-atime n
- *匹配0-n个，？匹配一个
- ![image-20220726210258282](https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20220726210258282.png)
  - -exec 执行
  - ls -l（执行啥）
  - { } 代表吧前面执行的结果（find的结果）填入此处（中间可以不用空格）
  - \; 分号代表结束，使用\转义
  - -exec改为-ok，此时执行前带询问
- grep找文件内容
  - grep -r 'str' ./ -n 在./目录下的文件中找带str的行
    - -n 同时显示行数
  - 记得默认其实不需要参数
  - -n不是指-name
- ps 
  - 一般是ps aux，原因如下
    - a所有用户的进程
    - u详细信息
    - x包含无控制终端的进程（后端进程）
  - ps aux | grep root
    - 搜索root的进程
    - 记得并不需要参数
- |与xargs
  - ![image-20220726210258282](https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20220726210258282.png)
    - find ./ -type f | ls -l
      - **此时上面这条命令，与单独执行ls -l没有区别，传参失败**
  - -exec的替代方案 1：xargs（配合|使用）
    - find ./ -type f | xargs ls -l
      - 使用xargs有效传参
      - 区别：<img src="https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20220726212356908.png" alt="image-20220726212356908" style="zoom: 25%;" />
        - xargs是等待前命令执行一句就执行一次，-exec是等待全部执行结束
        - 理论上结果搜索量大时，xrags可以分片映射，效率更高
      - 若文件名存在空格，xrags执行会失败
        - 强制使用null作为分隔符-print0打印
          - find ./ -type f -print0 | xargs -print0 ls -l
          - find ./ -type f -print0 | xargs -0 ls -l也可以
          - 前后都要加

### 软件包管理工具apt

- 双数版是长期支持版，单数板是短期支持版
- sudo apt **show** tree
  - 展示某个软件包是否安装，依赖等信息
- dpkg
  - dpkg -i xxx.deb
    - -i 安装
    - -r可以卸载（但是为啥不用remove呢（
- 源码安装的逻辑
  - ![image-20220726214807264](https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20220726214807264.png)

### 压缩

- gzip 压缩指令

- tar （可以间接调用gzip,bzip2完成压缩）

  - 打包指令
    - tar zcvf xxx.tar.gz xxxx(一吨文件)——gzip
    - tar jcvf  xxx.tar.gz xxxx(一吨文件)——bzip2
  - 解包指令
    - tar zxvf xxx.tar.gz ——gzip
    - tar jxvf  xxx.tar.gz ——bzip2

- rar

  - rar a -r xxx.rar xxxx(一吨文件)——压缩
    - 软连接无法压缩进去
  - unrar x xxx.rar ——解压

- zip

  - zip -r xxx.zip  xxxx(一吨文件)——压缩
  - unzip xxx.zip ——解压

  

### 其他指令

- who 连接到本电脑的用户情况
- job 显示现在后台进行的shell
- fg 放到前台运行
- bg 放到后台运行
- kill 根据PID杀进程
- env 显示所有环境变量
- top 带变化的任务管理器
- man看手册
  - ![image-20220727005141720](https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20220727005141720.png)
    - 如：man 3 printf 查printf库调用方式
    - 如：man 5 passwd 查询配置文件里每一个参数的含义
- clear 清屏
  - **Ctrl+L其实更好**
- alias 别名系统
  - 如给ps aux指令给个别名：
  - ![image-20220727005613595](https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/image-20220727005613595.png)
- echo回显
- umask 指定用户创建的文件的掩码（权限码）
  - 这里有点复杂，实际新建文件的权限是777-umask，若此时减去111有意义或部分有意义，则减去111或部分减1（系统认为程序创建后无可执行权限）
    - 如umask是002，新建的文件权限就是664（减了111）
    - 如umask是511，新建的文件权限是266（没有减去111）
    - 如umask是522，新建的文件权限是244（部分减1）
  - umask 002 ——修改为002
- Ctrl+T 终端+1
- Ctrl+Shift+T 终端在本窗口+1







​			