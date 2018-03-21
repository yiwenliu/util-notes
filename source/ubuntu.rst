Ubuntu
========
在登录界面无法进入桌面
------------------------
20180321，配有GPU的机器被人异常关机后，重新开机，在登录界面循环，无法进入系统桌面。

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

- 查看登录异常文件(failed)

ctrl+alt+f1进入控制台，在家目录下查看.Xauthority和.xsession-errors文件，应该和登录时间相符，删除.Xaut*, 后者记录了登录异常，可以直接打开该文件。

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

