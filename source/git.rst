Git
=====
场景
-----
在windows上会做一些调试代码的工作，然后，要同步到github上去。
在学习AI时，程序真正的运行都在ubuntu机器上（GPU）

git for windows
---------------------
安装
^^^^^^
安装可见《github入门与实践》p21，完成后，会有三个程序：

.. image:: _images/git-1.png

生成rsa
^^^^^^^^^
在git bash中执行

.. image:: _images/git-2.png

启动ssh-agent
^^^^^^^^^^^^^^^
1. 在git bash中执行

$eval $(ssh-agent -s)

2. 将SSH私钥添加到 ssh-agent

.. image:: _images/git-3.png

Q&A
------
git push时需要输入github的account
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
It is my understanding that GitHub has switched to using TLS 1.2, which is causing this error when your program is still trying to connect to GitHub using TLS 1.0.

重新安装最新版本的Git即可

认证Github时报错
^^^^^^^^^^^^^^^^^^
ubuntu和Github进行链接认证时报错，如下图

.. image:: _images/git-4.png

解决方法：$ssh-keyscan -t rsa github.com >> ~/.ssh/known_hosts