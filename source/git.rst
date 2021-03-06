Git
=====
场景
-----
在windows上会做一些调试代码的工作，然后，要同步到github上去。
在学习AI时，程序真正的运行都在ubuntu机器上（GPU）

.. _git-win:

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

远程仓库
----------------------------

添加远程仓库
^^^^^^^^^^^^^^^^
在本地仓库（.git目录）中可以记录有多个远程仓库

$git clone 仓库地址  # 被克隆的那个仓库自动记录为远程仓库，并且取名为origin

$git  remote add 仓库标识 仓库地址  # 使用github上仓库的SSH地址，而不是HTTPS地址。

修改远程仓库
^^^^^^^^^^^^^
.. code-block:: none
	:linenos:

	$git remote remove origin #首先删除原来的
	$git remote add xxx

查看远程仓库
^^^^^^^^^^^^^^^
$git remote -v

$git remote show <标识符>   # 查看远程仓库的信息可以使用

仓库&工作区
-----------------
将.git目录(本地仓库)的父目录中的内容称为“附属于该仓库的工作树”。

.. image:: _images/git-5.png

图解Git
----------
https://marklodato.github.io/visual-git-guide/index-zh-cn.html

版本
------------

版本库
^^^^^^^^^^^^
版本库是.git

提交版本
^^^^^^^^^^^^^
git add和git commit的实际作用是和Git的结构设计紧密相关，详见廖雪峰的工作区和暂存区

.. image:: _images/git-add.png

.. image:: _images/git-commit.png

这篇文章解答了我的几个疑惑：

1. 只是修改的文件，也需要先add，再commit,而不是可以直接commit
2. 可以执行多个add，最后执行一个commit
3. add和commit的操作都是针对本地git仓库的
4. Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
5. commit后“暂存区”就空了

commit背后的细节
++++++++++++++++++++
1. 一次commit就是一个版本，在版本库中形成了时间线，如下图所示：

.. image:: _images/git-6.jpg

2. HEAD指向当前commit的版本。

3. 一个版本有两个基本属性，commit-id和commit message

查看历史版本commit-id
^^^^^^^^^^^^^^^^^^^^^^^

$ git log  # 返回从HEAD指向的版本开始之前提交的版本

$ git relog  # 返回所有commit提交的版本

版本回退
^^^^^^^^^^
git reset
+++++++++++++++
可以选择性的改变如下参数：

- HEAD指针
- 分支指针
- 工作目录
- stage（index区）

详见https://marklodato.github.io/visual-git-guide/index-zh-cn.html#reset

git checkout
+++++++++++++++++++
$git checkout <commit_id> 来切换到指定的某一次提交，改变了HEAD指针，但是，分支指针不变

.. image:: _images/git-10.png

如此会造成HEAD指针游离。HEAD游离的弊端会在commit时体现出来，以及解决办法见https://juejin.im/post/5b2a30f96fb9a00e7a3d5724

HEAD指针&分支指针
-----------------------
HEAD指针&分支指针是Git中非常重要的概念

.. image:: _images/git-7.png

- HEAD指针就是.git/HEAD文件

.. image:: _images/git-8.png

- 分支指针就是.git/refs/heads目录下分支同名文件，文件内容就是某个commit id

$ cat develop
caada2821ae7adfec482c8f9f0eca21c764a6425

标签
------------
参考链接
^^^^^^^^^
https://www.liaoxuefeng.com/wiki/896043488029600/900788941487552

tag和commit的关系
^^^^^^^^^^^^^^^^^^^^^^^^
发布一个版本(commit)时，我们通常先在版本库中打一个标签（tag），标签也是版本库的一个快照。

tag默认是打在HEAD指向的版本上的，当然，也可以打在指定的commit-id版本上。


git管理的文件的状态
---------------------
从来没有add/commit的文件：Untracked files

add后：Changes to be committed

commit后：nothing to commit, working tree clean

commit后又修改的：changes not staged for commit

撤销修改
-----------
参考了https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374831943254ee90db11b13d4ba9a73b9047f4fb968d000

1. 修改后没有执行add，使用命令把工作目录的修改的文件还原。

$ git checkout -- readme.txt

2. 修改后执行了add


从远程仓库同步代码
------------------
当要从远端仓库获取代码时，pull, clone, fetch到底使用哪一个呢？

pull
^^^^^^
PULLis a high-level request that runs ‘fetch’ then a ‘merge’ by default,

.. code-block:: none
	:linenos:

	%> git checkout localBranch
	%> git pull origin master
	%> git branch
	master
	* localBranch

The above will merge the remote “master” branch into the local “localBranch”.

如果pull时，merge出错的解决办法请参考：https://help.github.com/cn/github/collaborating-with-issues-and-pull-requests/resolving-a-merge-conflict-using-the-command-line#competing-line-change-merge-conflicts

fetch
^^^^^^^
$ git fetch [remote-name][分支名]

1. 执行这个命令时要注意，本地哪里来的remote-name记录，途径：

  1）$git remote add手工添加

  2）之前执行过$git clone，自动取名的

2. 这个命令执行完成后，它们将被保存在本地的 "remote-name /分支名" 分支，
可以通过$git branch -a查看

clone
^^^^^^^
$ git clone <版本库的网址>

在一个目录下执行$git clone操作后完成了以下操作：

- 新建了一个远端仓库版本库同名的子目录, 如果要指定不同的目录名，可以将目录名作为git clone命令的第二个参数,$ git clone <版本库的网址> <本地目录名>
- 不需要执行$git init，在新建的子目录下就会有.git文件夹
- 这个远端仓库可以使任意的github上的仓库，不必先folk到自己账户下，然后在执行clone
- 自动给远端仓库取名为“origin”
- 这个命令可以相当于执行了如下两个命令

$git remote add
$git fetch

配置文件不受git管理
--------------------
1. 下载python.gitignore模板, `link <https://github.com/yiwenliu/gitignore/blob/master/Python.gitignore>`_
2. 下载并重命名为.gitignore到.git/同级目录下
3. 编辑.gitignore

分支
---------
查看本地分支
^^^^^^^^^^^^^^
$ git branch

- 当前分支前面会标一个*号

查看本地和远程分支
^^^^^^^^^^^^^^^^^^^^^
$ git branch -a

创建分支
^^^^^^^^^^
$ git branch dev

切换到已有分支
^^^^^^^^^^^^^^^^^^^
$ git switch <branch_name>

- 这个操作实际上，只是改变了.git/HEAD文件的内容

.. image:: _images/git-9.png

创建并切换分支
^^^^^^^^^^^^^^^
$ git switch -c dev

合并
^^^^^^
$ git merge <name>

git merge命令用于合并指定分支(dev)到当前分支。什么叫“当前分支”——应该是HEAD指向的分支

删除
^^^^^^^
$git branch -d <name>

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