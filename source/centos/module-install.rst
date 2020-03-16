(centos)OS组件安装
=========================
ssl
-----
在newsbrief服务器上运行程序时，会报错No module named '_ssl'。

有两个方法安装ssl，用yum安装或者从源码安装，前者安装后，还是没有/usr/local/ssl这个目录，用后者成功了。

1. 从https://www.openssl.org/source/上下载，再传到虚拟机上。
pscp C:\Users\yiwen\Downloads\openssl-1.1.1a.tar.gz root@172.22.27.85:/root/downloads/

2. 

.. code-block:: none
	:linenos:

	tar -xzvf openssl-1.1.1a.tar.gz
	cd openssl-1.1.1a/
	./config --prefix=/usr/local/ssl --openssldir=/usr/local/openssl
	make && make install

python3
-----------
1. 用PC从http://172.21.95.200/xrepos/python/上下载了.tar.gz包

2. 把gz包传到虚拟机

进入putty在pc上的安装路径

.. code-block:: none
	:linenos:


	c:\Program Files\PuTTY>pscp C:\Users\yiwen\Downloads\Python-3.6.2.tgz root@172.22.27.85:/root/downloads/

	c:\Program Files\PuTTY>pscp C:\Users\yiwen\Downloads\python.3.6.2.centos7.bin.tar.gz root@172.22.27.85:/root/downloads/

3. 安装参考链接： https://segmentfault.com/a/1190000009922582

注意，其中，建立了python3的符号链接

1. 
# yum install wget gcc make

2. 
#tar -xvf Python-3.6.1.tar

3.修改Python3.6.1/setup.py

search_for_ssl_incs_in = [
                          '/usr/local/ssl/include',
                          '/usr/local/ssl/include/openssl', # 添加此行，否则可能会报错找不到 rsa.h 文件
                          '/usr/contrib/ssl/include/'

4.修改Python3.6.1/Module/Setup.dist文件,注释掉下面5行：

.. code-block:: none
	:linenos:

	_socket socketmodle.c

	# Socket module helper for SSL support; you must comment out the other
	# socket line above, and possibly edit the SSL variable:
	SSL=/usr/local/ssl
	_ssl _ssl.c \
	    -DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \
	    -L$(SSL)/lib -lssl -lcrypto

5. 编译

.. code-block:: none
	:linenos:

	--prefix 是预期安装目录

	cd Python-3.6.1

	//否则，报炸不到libssl.so
	#echo 'export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/ssl/lib ' >> ~/.bashrc 
	#source ~/.bashrc
	./configure --prefix=/usr/local/python3.6
	make
	make install

6.
ln -s /usr/local/python3.6/bin/python3 /usr/bin/python3

7. 安装路径/usr/local/python3.6

pip
-------------
安装
^^^^^^^
#yum install python-pip python-wheel

更新pypi源
^^^^^^^^^^^
1. 麻烦平台部打开虚拟机访问http://172.20.85.12/pypi/srv/pypi/web/simple/

2. 可以在编辑CentOS shell账户Home目录下pip配置文件，vi ~/.pip/pip.conf文件，内容如下：

.. code-block:: none
	:linenos:

	[global] 
	index-url = http://172.20.85.12/pypi/srv/pypi/web/simple/
	[install]
	trusted-host=172.20.85.12

这样就是将默认的pypi源改成融发内部Pypi镜像源了，而不用每次pip install的时候通过-i参数指定。

git
------
安装git
^^^^^^^^^^^
# yum info git

nginx
--------

参考链接：https://segmentfault.com/a/1190000007116797

1. 安装

#yum -y install nginx

2. 卸载

#rpm -e nginx 

#rpm -e --nodeps nginx //这个命令相当于强制卸载，不考虑依赖问题。

3. 查看安装路径

yum 在线安装会将 nginx 的安装文件放在系统的不同位置，可以通过命令 rpm -ql nginx 来查看安装路径，

4， 启动

.. code-block:: none
	:linenos:

	service nginx start #启动 nginx 服务
	service nginx stop #停止 nginx 服务
	service nginx restart #重启 nginx 服务

5.查看nginx安装目录

在shell中输入命令

# ps -ef | grep nginx

返回结果

root      4593     1  0 Jan23 ?   00:00:00 nginx: master process /usr/sbin/nginx

6.查看nginx.conf配置文件目录

在shell中输入命令

# nginx -t

返回结果

nginx: the configuration file /etc/nginx/nginx.conf syntax is ok

nginx: configuration file /etc/nginx/nginx.conf test is successful

7. 在centos上打开80端口访问

Centos7默认安装了firewalld，如果没有安装的话，可以使用 yum install firewalld firewalld-config进行安装。

.. code-block:: none
	:linenos:

	#systemctl status firewalld或者 firewall-cmd --state //查看状态
	#firewall-cmd --version //查看版本
	#firewall-cmd --get-active-zones //查看区域
	#firewall-cmd --zone=public --list-ports //查看指定区域所有打开的端口
	#firewall-cmd --zone=public --add-port=80/tcp(永久生效再加上 --permanent) //在指定区域打开端口（记得重启防火墙）
	#firewall-cmd --reload //重启防火墙