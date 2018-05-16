Linux中的库
-------------
库从本质上来说是一种可执行代码的二进制格式，可以被载入内存中执行。库分静态库和动态库两种。

静态库
^^^^^^^
这类库的名字一般是libxxx.a，xxx为库的名字。利用静态函数库编译成的文件比较大，因为整个函数库的所有数据都会被整合进目标代码中，他的优点就显而易见了，即编译后的执行程序不需要外部的函数库支持，因为所有使用的函数都已经被编译进去了。当然这也会成为他的缺点，因为如果静态函数库改变了，那么你的程序必须重新编译。

动态库
^^^^^^^^
这类库的名字一般是libxxx.M.N.so，同样的xxx为库的名字，M是库的主版本号，N是库的副版本号。当然也可以不要版本号，但名字必须有。相对于静态函数库，动态函数库在编译的时候并没有被编译进目标代码中，你的程序执行到相关函数时才调用该函数库里的相应函数，因此动态函数库所产生的可执行文件比较小。由于函数库没有被整合进你的程序，而是程序运行时动态的申请并调用，所以程序的运行环境中必须提供相应的库。

运行mesos时，出现过“cannot find shared object"的错误，这是找不到动态链接库的错误。在安装cuDNN时，也就是几个.so文件。

关于.so，linux中有这么几个点：

1. 用于加载动态库的加载程序

当应用程序需要使用动态链接库里的函数时，由ld.so负责加载。

- /lib/ld.so，针对a.out格式的二进制可执行文件
- ld-linux.so，针对ELF格式的二进制可执行文件

2. .so默认存放路径

 /usr/lib; /lib

3. .so路径环境变量

- LD_AOUT_LIBRARY_PATH（a.out格式）、
- LD_LIBRARY_PATH（ELF格式）

1）在shell中添加

export LD_LIBRARY_PATH=$LD_LIBRARY_PATH: /usr/local/lib

2）shell启动时默认添加

如果不想每次新启一个shell都设置LD_LIBRARY_PATH，可以编辑~/.bash_profile文件：
$ vi ~/.bash_profile

添加：
LD_LIBRARY_PATH=$ LD_LIBRARY_PATH:/usr/local/lib
export LD_LIBRARY_PATH

4. 动态库配置文件

- 如果无法修改环境变量，可以在/etc/ld.so.conf.d下面添加配置文件some_lib.conf（some_lib可以用动态链接库库名表示，如opencv可写成opencv.conf），并在其中写入lib_path，
- 保存过后ldconfig一下，更新缓存文件/etc/ld.so.cache，新的 library才能在程序运行时被找到。

5. 寻找.so文件的顺序

LD_LIBRARY_PATH     /etc/ld.so.conf     /usr/lib; /lib

6. 就是不管做了什么关于library的变动后，最好都ldconfig(动态链接库管理命令)一下，不然会出现一些意想不到的结果。不会花太多的时间，但是会省很多的事。