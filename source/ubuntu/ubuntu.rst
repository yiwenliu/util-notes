Ubuntu
========
Install Ubuntu 16.04 LTS
--------------------------
https://www.techonthenet.com/linux/sysadmin/ubuntu/install_16_04.php

多显卡矿机安装ubuntu
---------------------
在一天6显卡的矿机上运行windows+nicehash非常不稳定，经常死机，只能试试ubuntu+zcash。

用ubuntu16安装盘，在开机的黑白界面选择“install ubuntu"后，进不去安装系统的图形界面。

用ubuntu18试试，可以安装。

在登录界面无法进入桌面
------------------------
20180321，配有GPU的机器被人异常关机后，重新开机，在登录界面循环，无法进入系统桌面(login loop)

1. 以为是密码错误

从网上寻找了重制ubuntu密码的方法, `method1 <http://blog.topspeedsnail.com/archives/6042>`_, `method2 <http://www.linuxdiyf.com/linux/21513.html>`_, 但是依然停在登录界面。

仔细观察登录界面，当输入错误密码时，会提示“invalid password”，而输入正确密码时就没有这个提示了。于是，怀疑是密码是对的，但是为什么进不了桌面呢？

2. ubuntu desktop故障

使用ctrl + alt + f1 ~f6进入ubuntu的字符界面，验证了密码的正确性， ctrl + alt +f7返回GUI。

网上关于密码正确进不去系统桌面的解决方案主要有两个：

- 重装desktop(failed)

.. code-block:: python
	:linenos:

	sudo apt-get update  
	sudo apt-get install --reinstall ubuntu-desktop  
	sudo apt-get install unity  
	sudo shutdown -r now  

- 查看登录异常文件(OK)

ctrl+alt+f1进入控制台，在家目录下查看.Xauthority和.xsession-errors（记录了登录异常，可以直接打开该文件）文件，应该和登录时间相符，

$rm -f .Xaut*

只执行这条命令还是无法进入桌面，考虑到非正常关机，卸载了NVIDIA的驱动后，就可以进去桌面了。

- startx(failed)

进入tty1, 运行startx

- 磁盘满(no trying)

`cleat boot space <http://www.wangmingkuo.com/linux/ubuntu-%E6%A1%8C%E9%9D%A2%E8%BF%9B%E4%B8%8D%E5%8E%BB-%E5%BE%AA%E7%8E%AF%E5%87%BA%E7%8E%B0%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2/>`_, 我使用$df -hl查看，磁盘并没有满啊

配置固定IP
------------
1. 修改NetworkManager.conf

$sudo gedit /etc/NetworkManager/NetworkManager.conf

将“managed=false”修改为“managed=true”。意思是，将网络连接设置为自定义或手动。

$sudo service network-manager restart #重启network manager

因为，如果Ubuntu系统采用的是desktop版，由于desktop版安装了NetworkManager，修改完interfaces文档中的内容后，不会生效。需要先修改/etc/NetworkManager/NetworkManager.conf文档中的managed参数，使之为true，并重启系统， 然后在修改/etc/network/interfaces文件，设置静态IP

2. `修改interfaces文件 <https://www.jianshu.com/p/d69a95aa1ed7>`_

X window system
-----------------
在安装nvidia驱动的过程中，经常会碰到X display, X server, X client等概念，于是就想搞清楚X windows system.

1. X系统有四大组件：

.. image:: ../_images/ubuntu-1.png

X server
^^^^^^^^^^^^^^^^^^^

Xwindow 系统服务器端，通过驱动程序（硬件规范）来管理硬件资源，那么 X Server 管理的设备主要有哪些呢？其实与输入/输出有关喔！包括键盘、鼠标、手写板、显示器 (monitor) 、萤幕解析度与色彩深度、显卡 (包含驱动程序) 与显示的字型等等，都是 X Server 管理的。

 X Server 还有一个重要的工作，那就是将来自输入装置 (如键盘、鼠标等) 的动作告知 X Client， 但是 X Server 本身并不知道周边设备这些动作会造成什么显示上的效果， 因此 X Server 会将周边设备的这些动作行为告知 X Client ，让 X Client 去伤脑筋。

注意，如果通过putty连接时使用的是ssh，而不是X windows系统 

X client
^^^^^^^^^^^^^^^^^^^
登录ubuntu后，在其桌面上使用的openoffice, firefox, 图形化的管理界面都是X Client软件。由ubuntu的桌面环境来提供。我们也称呼 X Client 为 X Application (X 应用程序)。

桌面环境的重要组成部分——X Window Manager，一个特殊的 X Client ，负责管理上述所有的 X client 软件。

X Window Manager
^^^^^^^^^^^^^^^^^^^
Window Manager 是一种特殊的 Xclient,用于控制窗口程序的位置和外观，常说的GNOME, XFEC桌面就属于“窗口管理员”。使用窗口管理器时，Xserver 并不直接与其它 Xclient 通信，而是通过 WM 中转，当一些消息被定义为 WM 指令时，它们会被拦截。例如 在ubuntu桌面上打开了数个窗口（x client程序），用户点击了一下鼠标，X server捕捉到这个输入，把这个信号传给windows manager，由其判断，哪个是当前窗口，再把这个信号传给对应的X client程序，再有X client程序决定在当前位置点击鼠标是什么意义并产生绘图的数据，经由windows manager传递给X server来显示。

Display Manager
^^^^^^^^^^^^^^^^^^^

2. 这四个组件的工作流程可以参见 

- `x server&x client <https://i.linuxtoy.org/docs/guide/ch19s03.html>`_ 
- `x window manager <https://i.linuxtoy.org/docs/guide/ch19s04.html>`_
-  `理解 Xwindow <http://wiki.ubuntu.org.cn/%E7%90%86%E8%A7%A3_Xwindow>`_

渲染的概念
------------
渲染（英语：render，或称“绘制”）在电脑绘图中，是指：用软件从模型生成图像的过程。模型是用语言或者数据结构进行严格定义的三维物体或虚拟场景的描述，它包括几何、视点、纹理、照明和阴影等信息。图像是数字图像或者位图图像。渲染用于描述：计算视频编辑软件中的效果，以生成最终视频的输出过程。通常依靠图形处理器（GPU）完成这个过程。

OpenGL
--------
OpenGL（英语：Open Graphics Library，译名：开放图形库或者“开放式图形库”）是用于渲染2D、3D矢量图形的跨语言、跨平台的应用程序编程接口（API）。

OpenGL规范由1992年成立的OpenGL架构评审委员会（ARB）维护,NVIDIA是成员之一。

GLX
-----
GLX (initialism for "OpenGL Extension to the X Window System") is an extension to the X Window System core protocol providing an interface between OpenGL and the X Window System as well as extensions to OpenGL itself. It enables programs wishing to use OpenGL to do so within a window provided by the X Window System. 

GLX consists of three parts:

- An API that provides OpenGL functions to an X Window System application.
- An extension of the X protocol, which allows the client (the OpenGL application) to send 3D rendering commands to the X server (the software responsible for the display). The client and server software may run on different computers.
- An extension of the X server that receives the rendering commands from the client and passes them on to the installed OpenGL library

If client and server are running on the same computer and an accelerated 3D graphics card using a suitable driver is available, the former two components can be bypassed by DRI. In this case, the client application is then allowed to directly access the video hardware through several API layers.

GLX distinguishes two "states": indirect state（上图） and direct state（下图）.

.. image:: ../_images/Linux_graphics_drivers_Utah_GLX.svg.png

（上图）Indirect rendering over GLX, using Utah GLX

.. image:: ../_images/Linux_graphics_drivers_DRI_early.svg.png

（上图）early Direct Rendering Infrastructure

配置ubuntu使用git
-------------------
在windows上要调试程序，在有GPU的ubuntu上运行，两者的代码要同步，借助github

和 :ref:`git for windows <git-win>` 大多的步骤类似，在测试ssh链接前要执行下列步骤：

$ssh-keyscan -t rsa github.com >> ~/.ssh/known_hosts

否则，ssh -T git@github.com时会报错，如下图

.. image:: ../_images/ubuntu-2.png