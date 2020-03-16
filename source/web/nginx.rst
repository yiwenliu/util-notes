(web)Nginx
==============
配置nginx为静态文件服务器
--------------------------
1. 修改nginx配置文件/etc/nginx/nginx.conf

.. code-block:: none
	:linenos:

	#1. 在server{}中添加如下
	location /audio/ {
	            root /root/; #对应的本地目录是/root/audio
	            autoindex on;
	        }
	#2. 把第一行改为user root;而不是user nginx;因为要访问/root

2. 重启nginx