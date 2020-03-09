Http Proxy
=============
Tinyproxy
-----------
文档
^^^^^^
https://tinyproxy.github.io/

安装
^^^^^^^
yum -y install tinyproxy

修改配置
^^^^^^^^^^
修改/etc/tinyproxy/tinyproxy.conf中的如下几点：

1. 修改端口号，配置文件第23行，内容如下：
.. code-block:: none
	:linenos:

	Port 8001  #注意，虚拟机上允许pc访问的端口是8000-8120

2. 修改允许访问的IP，配置文件第211行，内容如下：

.. code-block:: none
	:linenos:

	Allow 127.0.0.1
	将127.0.0.1修改为使用这个代理的客户机的IP，如果你想任何人都可以访问，把这行前面加个#注释掉就可以了

开启防火墙端口
^^^^^^^^^^^^^^^^^^
Centos升级到7之后，内置的防火墙已经从iptables变成了firewalld。所以，端口的开启还是要从两种情况来说明的，即iptables和firewalld。

firewall
+++++++++++
- 显示状态： $firewall-cmd --state
- 查看所有打开的端口： $firewall-cmd --zone=public --list-ports

.. code-block:: none
	:linenos:

	$firewall-cmd --zone=public --add-port=8001/tcp --permanent
	$firewall-cmd --reload

运行
^^^^^^^
systemctl是CentOS7的服务管理工具中主要的工具，它融合之前service和chkconfig的功能于一体。

.. code-block:: none
	:linenos:

	$systemctl status tinyproxy  #查看状态
	$systemctl enable tinyproxy #设置开机启动
	$systemctl start tinyproxy
	$systemctl stop tinyproxy  #停止

日志
^^^^^^^
/var/log/tinyproxy/tinyproxy.log