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

python的魔术方法
-----------------
Reference Link
^^^^^^^^^^^^^^^^^
http://pycoders-weekly-chinese.readthedocs.io/en/latest/issue6/a-guide-to-pythons-magic-methods.html

What is magic method
^^^^^^^^^^^^^^^^^^^^^
python的魔术方法就是被__环绕的方法，最典型的就是__init__()。

python魔术方法是python面向对象的一切。

How to call magic method
^^^^^^^^^^^^^^^^^^^^^^^^^
注意，在客户端代码中有固定的写法用来告知程序自动调用相应的魔术方法。

.. image:: _images/python-magic-method.png

python为什么要引入魔术方法
^^^^^^^^^^^^^^^^^^^^^^^^^^
使用Python的魔术方法的最大优势在于他们提供了一种简单的方法来让对象可以表现的像内置类型一样。

下面章节中将要涉及的“迭代器”，“会话控制器”等无不实践了这一原则。

protocol
^^^^^^^^^^
python中的协议是指为了创建定制序列，class必须要实现的魔术方法。例如，下节所讲的“迭代协议”。

可以调用的对象
^^^^^^^^^^^^^^^
允许一个类的实例像函数一样被调用。实质上说，这意味着 x() 与 x.__call__() 是相同的。

.. code-block:: python
	:linenos:

	class Entity:
	'''调用实体来改变实体的位置。'''

	def __init__(self, size, x, y):
	    self.x, self.y = x, y
	    self.size = size

	def __call__(self, x, y):
	    '''改变实体的位置'''
	    self.x, self.y = x, y

会话控制器
-----------
Definition
^^^^^^^^^^^^
一个会话控制器就是一个实现了 __enter__(self)和 __exit__()两个魔术方法的class。

最常使用的就是如下代码。open()返回了一个IOBase obj，在class IOBase定义中就实现了上述两个魔术方法。

.. code-block:: python
	:linenos:

	with open('foo.txt') as bar:
	  # perform some action with bar
	  pass

with语句的执行过程
^^^^^^^^^^^^^^^^^^^
紧跟with的对象的__enter__ 的返回值被 with 语句的目标或者 as 后的名字绑定。

自定义会话控制器
^^^^^^^^^^^^^^^^^^
.. code-block:: python
	:linenos:

	class Closer:
	'''通过with语句和一个close方法来关闭一个对象的会话管理器'''

	def __init__(self, obj):
	    self.obj = obj

	def __enter__(self):
	    return self.obj # bound to target

	def __exit__(self, exception_type, exception_val, trace):
	    try:
	        self.obj.close()
	    except AttributeError: # obj isn't closable
	        print 'Not closable.'
	        return True # exception handled successfully

客户端代码如下：

.. code-block:: none
	:linenos:

	>>> from magicmethods import Closer
	>>> from ftplib import FTP
	>>> with Closer(FTP('ftp.somesite.com')) as conn:
	...     conn.dir()
	...
	>>> conn.dir()
	>>> with Closer(int(5)) as i:
	...     i += 1
	...
	Not closable.
	>>> i
	6

Iterator(迭代器)
-------------------
Intro
^^^^^^
iterator是一种“设计模式”，和“观察者模式”、“访问者模式”同属于“面向任务的模式”，用于“执行及描述任务”。

各种语言实作迭代器的方式皆不尽同，有些面向对象语言像Java, C#, Ruby, Python, Delphi都已将迭代器的特性内建语言当中，完美的跟语言整合，我们称之隐式迭代器（implicit iterator）

迭代器另一方面还可以整合生成器（generator）。有些语言将二者视为同一界面，有些语言则将之独立化。

Reference
^^^^^^^^^^^
https://www.programiz.com/python-programming/iterator

迭代协议
^^^^^^^^^
Technically speaking, Python iterator object must implement two special methods, __iter__() and __next__(), collectively called the **iterator protocol**, 例子可见下一小节。

- __iter__()

一个实现这个成员方法的iterator就把自身也变成了一个iterable，就可以使用在 **for loop** 中了。见下面的小节“How for loop actually works”

Built-in functions using iterable in Python
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
python很对的内建的函数的参数都是iterable，例如

- sum(iterable[, start])
- max(iterable, \*iterables[,key, default])
- for element in iterable

what is iterable&iterator
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Iterator in Python is simply an object that can be iterated upon. An object which will return data, one element at a time. In the other words, iterator必须实现__iter__()和__next__()

An object is called iterable if we can get an iterator from it throught iter() function.In the other words, iterable只需要实现__iter__()

built-in iterable containers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Most of built-in containers in Python like: list, tuple, string etc. are iterables.

.. code-block:: none

	>>> x = [42,23,24]
	#从可迭代(iterable)对象中获取iterator
	#iter() in turn calls the __iter__() method
	>>> it = iter(x)
	>>> type(it)
	<class 'list_iterator'>
	>>> type(x)
	<class 'list'>
	#We use the next(), which in turn calls the __iter__() method, to manually iterate through all the items of an iterator. 
	#When we reach the end and there is no more data to be returned, it will raise StopIteration. 
	>>> next(it)
	42
	#next(obj) is same as obj.__next__()
	>>> it.__next__()
    23
    #built-in iterable container不能被next()直接调用
    >>> next(x)
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	TypeError: 'list' object is not an iterator

Building Your Own Iterator in Python
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
1. 自定义iterator，就必须要满足“迭代协议”

2. Can build your own iterables in python?

3. 既然The __iter__() method returns the iterator object itself，客户端的使用iterator的代码又是next(iter(iterator))，那么为什么必须实现__iter__()呢，可以直接使用next(iterator)吗？

上面两个问题的答案是相同的，“实现了__iter__()的iterator自身就是一个iterable”，就可以使用在for循环中了。

4. Python generators are a simple way of creating iterators. 

How to use your own iterator
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
从客户端代码看，有两种使用方法，可以参考reference-Building Your Own Iterator in Python：

1. 第一步，new an iterator；第二步，next(the-new-iterator)
2. 第一步，new an iterator；第二步，for element in the-new-iterator

How for loop actually works
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
下面的代码中最重要的几点就是 

- iterable才能用到for语句中去
- element是next(iter_obj)的返回值

.. code-block:: none
	:linenos:

	for element in iterable:
	    # do something with element
	    pass

Is actually implemented as

.. code-block:: none
	:linenos:

	# create an iterator object from that iterable
	iter_obj = iter(iterable)
	# infinite loop
	while True:
	    try:
	        # get the next item
	        element = next(iter_obj)
	        # do something with element
	        pass
	    except StopIteration:
	        # if StopIteration is raised, break from loop
	        break

Why using iterator
^^^^^^^^^^^^^^^^^^^^^
iterator其实和定义一个函数以实现一个功能是相同的，为啥不定义一个函数算了呢？

- 设计模式中，惯用的伎俩就是把操作外化为类
- 语言提供统一的调用接口，iter(), next()

Generator
-----------
Reference
^^^^^^^^^^^
学习generator时主要参考了两个链接：

- `python生成器的优点-zhihu <https://www.zhihu.com/question/24807364>`_
- `Python Generators <https://www.programiz.com/python-programming/generator>`_

When to use generator
^^^^^^^^^^^^^^^^^^^^^^^
我的理解是只要生成list等sequence的地方，就可以考虑使用generator

Why generator when there's iterator
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
用定义函数来取代定义类，以简化代码。

How to define a generator
^^^^^^^^^^^^^^^^^^^^^^^^^^^
generator并不像iterator那样用class来定义，Simply speaking, a generator is a function that returns an object (iterator) which we can iterate over (one value at a time).

python中generator的两种定义方式：

1. 以前经常使用的列表推导式

- generator expression creates an anonymous generator function.
- using round parentheses.

.. code-block:: none

	>>> ge = (x**2 for x in [1,2,3])
	>>> ge
	<generator object <genexpr> at 0x00000000024B8AF0>
	>>> type(ge)
	<class 'generator'>

2. 生成器函数

If a function contains at least one yield statement (it may contain other yield or return statements), it becomes a generator function. 无论是有多个yield()语句，还是在for loop中调用yield()，例如 `reverse the string <https://www.programiz.com/python-programming/generator#with-loop>`_.

The difference is that, while a return statement terminates a function entirely, yield statement pauses the function saving all its states and later continues from there on successive calls.

.. code-block:: python
	:linenos:

	# A simple generator function
	def my_gen():
	    n = 1
	    print('This is printed first')
	    # Generator function contains yield statements
	    yield n

	    n += 1
	    print('This is printed second')
	    yield n

	    n += 1
	    print('This is printed at last')
	    yield n

generator function的执行过程
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
1. 使用next() 

.. code-block:: python
	:linenos:

	# It returns an iterator object but does not start execution immediately.
	>>> a = my_gen()

	# We can iterate through the items using next().
	# 代码从第3行开始执行至第一个yield语句, the function is paused and the control is transferred to the caller.
	#yield后表达式的值被返回给caller
	>>> next(a)
	This is printed first
	1

	# Local variables and theirs states are remembered between successive calls.
	# my_gen()从上次挂起的地方继续执行至第二个yield语句
	>>> next(a)
	This is printed second
	2

	>>> next(a)
	This is printed at last
	3

	# Finally, when the function terminates, StopIteration is raised automatically on further calls.
	>>> next(a)
	Traceback (most recent call last):
	...
	StopIteration
	>>> next(a)
	Traceback (most recent call last):
	...
	StopIteration

2. 使用for loop

.. code-block:: none
	:linenos:

	# my_gen() return an iterable iterator
	# item就是my_gen()中每一条yield语句后表达式的值
	for item in my_gen():
	    print(item)
	#输出如下
	This is printed first
	1
	This is printed second
	2
	This is printed at last
	3    

Why using generator not iterator class
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Suppose we have a log file from a famous fast food chain. The log file has a column (4th column) that keeps track of the number of pizza sold every hour and we want to sum it to find the total pizzas sold in 5 years.
`Pipelining Generators <https://www.programiz.com/python-programming/generator#use>`_

Attention
^^^^^^^^^^
生成器只能遍历一次

Closure
---------
Reference link
^^^^^^^^^^^^^^^^
https://www.programiz.com/python-programming/closure

Python为什么要引入closure概念
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
- 为了替代类。众所周知，class封装了属性和方法，方法除了使用被调用时传入的形参，就只能访问class中定义的属性了。closure就是为了实现类似“类方法”的作用。
- Closures can avoid the use of global values and provides some form of data hiding.

How to create a closure
^^^^^^^^^^^^^^^^^^^^^^^^^
`Defining a Closure Function <https://www.programiz.com/python-programming/closure#define>`_, `When do we have a closure <https://www.programiz.com/python-programming/closure#when>`_

**Q**：一个class可以定义多个方法，那么在一个enclosing function中是否可以定义多个nested function(closure)？如何决定返回哪一个呢？
**A**:But when the number of attributes and methods get larger, better implement a class.

How does a closure work
^^^^^^^^^^^^^^^^^^^^^^^^
1. call the enclosing function with some parameters, so the closure can take the passing parameters and the nonlocal variable as **Initialization Parameters** just like which are passed when creating an object but has not been called yet
2. call the closure to run the closure body

what the function really is in python
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
- Besides that function is an object, even everything in Python (Yes! Even classes) are objects. 

- Various different names can be bound to the same function object.

.. code-block:: python
	:linenos:

	def first(msg):
	    print(msg)    

	first("Hello")

	second = first
	second("Hello")

- Functions can be passed as arguments to another function.

- a function can return another function, like the closure

- In fact, any object which implements the special method __call__() is termed callable. So, in the most basic sense, a decorator is a callable that returns a callable.

Decorator
----------
python为什么要引入decorator的概念
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
在设计模式中有Decorator这一模式，用来生成对象，特点是有一个核心类和一堆装饰类。

Decorators in python is to add functionality to an existing code(function). 

- **Q**:对于一个不能修改函数体的现存的函数，如何才能给它增加功能呢？

**A**:把decorated function作为参数传入decorator

- **Q**:既然以decorated function作为形参，decorator为什么不在其函数体中，在调用decorated function前后，实现它想增加的功能就可以了呢，为什么还要以closure的方式返回一个函数对象呢？

**A**:因为decorated 客户function实现的是核心功能，所以在“客户端代码”中，还是要出现调用decorated function的形式，decoreated_function_name()

How to use
^^^^^^^^^^^^
1. 第一步，定义一个decorator function

- 如何区分decorator和普通function呢？以function作为参数的就是decorator
- 注意：其中nested function的形参必须和decorated functin相同，因为decorator要返回nested function object，那么如何定义一个可以修饰任意数量参数的“被修饰函数”的decorator呢？

.. code-block:: python
	:linenos:

	'''
	@args: will be the tuple of positional arguments and 
	@kwargs: will be the dictionary of keyword arguments.
	'''
	def works_for_all(func):
	    def inner(*args, **kwargs):
	        print("I can decorate any function")
	        return func(*args, **kwargs)
	    return inner

2. 第二步，对于decorated function使用decorator

.. code-block:: python
	:linenos:

	'''
	@make_pretty
	def ordinary():
	    print("I am ordinary")

	is equivalent to

	def ordinary():
	    print("I am ordinary")
	ordinary = make_pretty(ordinary)
	'''
	@decorator_name
	def decorated_function_name():
	  pass

3. 第三步，call the decorated function 

Examples
^^^^^^^^^^
1. `在除法运算前检查除数非零 <https://www.programiz.com/python-programming/decorator#decorating>`_

- 存在一个实现除法运算的核心函数
- 在运行核心功能前需要实现一个检查的功能

2. `decorator chain <https://www.programiz.com/python-programming/decorator#chaining>`_

- 上面这个链接中的例子像极了decorator design pattern

property
-----------
Reference Link
^^^^^^^^^^^^^^^^
https://www.programiz.com/python-programming/property

what is property
^^^^^^^^^^^^^^^^^
`property is a built-in function to return a property object <https://www.programiz.com/python-programming/property#dig>`_

python为什么要引入property
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
python的OO实现方式（无论类还是其实例都是object）对“实例属性”的操作太有随意性，没有对属性public, protected和private的访问权限控制，任何使用了某个class的代码，实例化一个class object后，可以给这个object进行任意属性的操作（添加、修改等），哪怕这个属性根本和class无任何关系，例如 `An Example <https://www.programiz.com/python-programming/property#eg>`_。

引入property的目的：

- 对操作实例属性必须加以控制，包括，参数检查、类型检查、读写限制等

注意：虽然可以定义set和get方法，例如在这段代码中 `Using Getters and Setters <https://www.programiz.com/python-programming/property#using>`_, 但是，仍然无法强制客户端代码使用这两个方法来读写 **_temperature** 属性

- 提供一种访问实例属性的统一的简单的方法

想要达到的效果是，客户端代码可以直接使用“对象.属性名”来读写属性，但是程序会自动执行对应的get, set函数。

Look inside the property
^^^^^^^^^^^^^^^^^^^^^^^^^^
1. 定义class时使用了decorator function property()把“类方法”变成了“类属性”，类实例通过访问这些“类属性”来间接操作对应的“实例属性”。

2. `使用property()的初级版本代码 <https://www.programiz.com/python-programming/property#power>`_ 可以看清楚上述原理。

- 最重要的代码是class Celsius定义的最后一行temperature = property()

- the actual temperature value is stored in the private variable _temperature. The attribute temperature is a property object which provides **interface** to this private variable.

- 代码中的temperature是一个property object（属性对象），是一个类属性吗？但是，从客户端代码的执行情况来看，又像是一个实例属性？因为在执行__init__()中的self.temperature = temperature语句时，set_temperature()被执行了？