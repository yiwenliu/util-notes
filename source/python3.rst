Python3
=========
读取命令行参数
---------------
从C语言时代就使用的方式

.. code-block:: python
	:linenos:

	import sys
	arg1 = sys.argv[1]

确实够简单，但是如果想实现如下功能，该怎么办呢？

- sys.argv[1]是使用"positional arguments"的一种，如果想给"positional arguments"取个名字，并用这个名字（key）来访问呢？
- 让命令行可以使用"optional arguments"，并用key来访问呢？
- 给命令行产生进行类型检查、设置默认值、设置取值范围？

这是，就该使用argparse module了， 
- `python argparse用法总结 <https://www.jianshu.com/p/fef2d215b91d>`_
- `官方文档 <https://docs.python.org/3/library/argparse.html>`_

python的高级特性
-----------------
有两个参考学习链接：

https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014317568446245b3e1c8837414168bcd2d485e553779e000

https://www.programiz.com/python-programming/iterator

Iterator(迭代器)
-------------------
iterator是一种“设计模式”，和“观察者模式”、“访问者模式”同属于“面向任务的模式”，用于“执行及描述任务”。

各种语言实作迭代器的方式皆不尽同，有些面向对象语言像Java, C#, Ruby, Python, Delphi都已将迭代器的特性内建语言当中，完美的跟语言整合，我们称之隐式迭代器（implicit iterator）

迭代器另一方面还可以整合生成器（generator）。有些语言将二者视为同一界面，有些语言则将之独立化。

迭代协议
^^^^^^^^^

.. code-block:: none
	:linenos:

	>>> x = [42,23,24]
	#从可迭代(iterable)对象中获取iterator
	>>> it = iter(x)
	>>> type(it)
	<class 'list_iterator'>
	>>> type(x)
	<class 'list'>
	#获取iterator指向的iterable object的内容
	>>> next(it)
	42