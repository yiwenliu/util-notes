Sphinx
=======

source/index.rst
------------------
the index document is the top node of the "content tree",The main function of the master document is to 

- serve as a welcome page, and to 
- contain the root of the “table of contents tree” (or toctree). This is one of the main things that Sphinx adds to reStructuredText, a way to connect multiple files to a single hierarchy of documents.

directives
------------
Intro
^^^^^^^^
Directives can have arguments, options and content.

.. code-block:: none

  .. directivename:: argument ...
     :option: value

     Content of the directive.

- Arguments are given directly after the double colon following the directive’s name. Each directive decides whether it can have arguments, and how many.
- Options are given after the arguments, in form of a “field list”. The maxdepth is such an option for the toctree directive.
- Content follows the options or arguments after a blank line. Each directive decides whether to allow content, and what to do with it.

A common gotcha with directives is that the first line of the content must be indented to the same level as the options are.


toctree
^^^^^^^^
在index.rst中toctree这个指令。

toctree is a reStructuredText directive, a very versatile piece of markup.

插入python代码
^^^^^^^^^^^^^^^^
.. code-block:: none

  .. code-block:: python
    :linenos:

    input = ...

插入图片
^^^^^^^^^^

.. code-block:: none

  .. image:: images/ball1.gif

插入内部链接
^^^^^^^^^^^^^
第1步，在需要链接的标题上方定义label

.. code-block:: none

  .. _label-name:   #必须有_开头的引用标签
                       #必须空行
  标题1

第2步，引用label

在需要链接的地方输入

.. code-block:: none

  :ref:`Link title <label-name>`  #不要写定义label-name时前面的_

使用下标
^^^^^^^^^^
操作符为 :rolename:`content`

- :durole:`subscript` – 下标
- :durole:`superscript` – 上标
