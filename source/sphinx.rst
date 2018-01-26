使用ReadtheDocs托管文档
=======================
采用Sphinx + GitHub + ReadtheDocs 作为文档写作工具，用以替代Evernote。
使用reStructuredText格式写作，Sphinx 生成文档，GitHub 托管文档，再导入到 ReadtheDocs。

Instllation
--------------
安装sphinx
^^^^^^^^^^^
参考了https://www.xncoding.com/2017/01/22/fullstack/readthedoc.html中的安装命令。

虽然是在windows+Anaconda环境中安装sphinx，但是仍然建议使用$pip install，而不是$conda install

工作流程
----------
1. 用sphinx初始化workspace

其实就是在一个目录中创建了一些make html时需要的文件和子目录，在"Anaconda Prompt"中执行如下命令

.. code-block:: none
  :linenos:

  # 创建文档根目录
  mkdir -p /root/work/scrapy-cookbook
  cd scrapy-cookbook/
  # 可以回车按默认配置来写
  sphinx-quickstart

在初始化的过程中，sphinx会提示确认如下信息：

- Separate source and build directories (y/n) [n]: y
- Project name: util-notes
- Author name: yiwen
- Project language: zh_CN
- Source file suffix:.rst/.txt
- autodoc: automatically insert docstrings from modules (y/n) [n]: y
- intersphinx: link between Sphinx documentation of different projects (y/n) [n]:y

初始化完成后，在root path中生成的子目录的作用如下：

- _build:place the build directory for sphinx output
- _templates: custom html templates
- _statics:custom stylesheets and other static files

2. 更改主题 sphinx_rtd_theme

参考https://www.xncoding.com/2017/01/22/fullstack/readthedoc.html

3.预览效果

参考https://www.xncoding.com/2017/01/22/fullstack/readthedoc.html

#. 在workspace中，用sublime写reStructuredText格式的笔记

#. 在github上创建仓库，命名为util-notes

#. 建立github仓库和sphinx root path的同步

在git bash下执行如下命令

.. conde-block:: none

  $cd D:\sphinx-docs\tensorflow-notes
  $echo "# tensorflow-notes" >> README.md #创建名为readme的markdown格式文件
  $git init  #初始化了空的Git仓库，新建了一个.git子目录，即仓库，存储着管理当前目录内容所需要的仓库
  数据，当前目录就是“附属于该仓库的工作树”，
  $git remote add origin https://github.com/yiwenliu/tensorflow-notes.git #设置本地仓库的远程仓库
  $git add .
  $git commit -m "first commit"
  $git push -u origin master


#. 建立ReadtheDocs和Github之间的hook

#. push笔记到Github，ReadtheDocs会自动更新

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