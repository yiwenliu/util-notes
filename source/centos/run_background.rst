配置后台运行程序
==================
1. 使用setsid

参考链接：https://www.ibm.com/developerworks/cn/linux/l-cn-nohup/index.html

.. code-block:: none
	:linenos:

	#启动虚拟环境
	#setsid python xxx.py
	#ps -aux |grep xxx //查看所有在后台运行的进程

2.使用nohup

nohup python yourscript.py > myout.file 2>&1 &

3. 脚本 start_newsbrief.sh (采用)

.. code-block:: none
	:linenos:

	#!/bin/bash
	cd /root/newsbrief
	source nb-env/bin/activate
	nohup python summary.py > newsbrief_out.file 2>&1 &

4. python脚本

首先，我必须创建一个shell脚本来包装“source”命令。这就是说，我使用了“.”。

.. code-block:: none
	:linenos:

	#!/bin/bash
	. /path/to/env/bin/activate

然后，从我的python脚本中，我可以这样做：

.. code-block:: none
	:linenos:

	import os
	os.system('/bin/bash --rcfile /path/to/myscript.sh')

最后，运行

nohup python yourscript.py > myout.file 2>&1 &