My First Miner
================
Hardware
----------
我有一台用来学习ML的机器，配置如下

- ubuntu 16
- Gtx 1080 ti
- i5

不用来训练NN的时候，闲置起来太浪费，那就来挖矿吧，^_^

Mining Software and Platform
------------------------------
- GPU可以挖什么矿呢

1. 从f2pool上，查到N卡可以挖的币种

- 尝试过以下这些挖矿形式

1. 接入f2pool矿池，但是其官网提供的方法都是基于windows的
2. 在Google查询时，看到有人用Nicehash，只是出卖算力来赚取bitcoin
3. Nicehash提供的miner也只有windows版本，但是在其官网上提供了 `在ubuntu上用N卡挖Zcash的文档 <https://www.nicehash.com/help/zcash-mining>`_ ，就这么干吧

Mining Zcash
---------------
Reference
^^^^^^^^^^
主要的两个参考链接是

- `Equihash miner for NiceHash on Github <https://github.com/nicehash/nheqminer>`_
- `Running equihash algorithm on NiceHash on Ubuntu <https://steemit.com/mining/@deadums/running-equihash-algorithm-on-nicehash-on-ubuntu>`_

Installing Boost
^^^^^^^^^^^^^^^^^^^
Boost一定要安装，但是，安装过程并不顺利。

1. (FAILED)using apt-get

.. code-block:: none
	:linenos:

	#安装成功，但是cmake时，报找不到静态库的错误
	>sudo apt-get install libboost-dev
	#报依赖错误
	>sudo apt-get install libboost-all-dev
	
2. (OK)本地编译安装

`Guide on the website <https://www.boost.org/doc/libs/1_60_0/more/getting_started/unix-variants.html#easy-build-and-install>`_

.. code-block:: none
	:linenos:

	#安装依赖
	$sudo apt-get install mpi-default-dev　　#安装mpi库  
	$sudo apt-get install libicu-dev　　　　　#支持正则表达式的UNICODE字符集   
	$sudo apt-get install python-dev　　　　　#需要python的话  
	$sudo apt-get install libbz2-dev zlib1g-dev

	#下载boost好了以后，解压 .bz2 文件
	$tar -jxvf xx.tar.bz2

	#解压之后，进入解压目录，执行：
	$./bootstrap.sh
	$sudo ./b2 install

Install nheqminer
^^^^^^^^^^^^^^^^^^^
在这一步中，有几点需要注意：

1. 我把nheqminer下载到了“~/zcash”下

2. 执行cmake时，要使用参数，否则会报错

.. code-block:: none
	:linenos:

	$cmake -DCUDA_CUDART_LIBRARY=/usr/local/cuda/lib64/libcudart.so ../nheqminer

Run NiceHash
^^^^^^^^^^^^^^
.. code-block:: none
	:linenos:

	$cd ~/zcash/nicehash
	$./nheqminer -l equihash.hk.nicehash.com:3357 -u YOUR_BTC_ADDRESS_HERE.worker_name -cd 0 -t 6