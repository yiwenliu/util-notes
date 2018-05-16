ubuntu下安装程序的三种方法
---------------------------
apt-get
^^^^^^^^^
- apt-get install xxx 安装xxx  。如果带有参数，那么-d 表示仅下载 ，-f 表示强制安装  
- apt-get remove xxx 卸载xxx  
- 卸载并清除配置命令： apt-get remove --purge softname1
- apt-get update 更新软件信息数据库  
- apt-get upgrade 进行系统升级  
- apt-cache search 搜索软件包  
- Tips：建议您经常使用“apt-get update”命令来更新您的软件信息数据库 

1. apt-get 下载的包存放在了哪里, /var/cache/apt/archives

2. 一个软件包安装完后，存放于系统的哪里——一般是分散的：

1）文档一般在 /usr/share
2）可执行文件 /usr/bin
3）配置文件 /etc
4）lib文件 /usr/lib
5）在环境变量中还有记录

3. apt-get解决了两个问题
1）网上仓库
2）程序间的依赖关系

4. apt-get的下载源

APT使用一个文件列出可获得软件包的镜像站点地址，这个文件就是/etc/apt/sources.list。

sources.list中每一行的格式

.. code-block:: none
	:linenos:

	deb uri distribution [component1] [componenent2] [...]
	deb-src uri distribution [component1] [componenent2] [...]

1) 每行的第一个单词deb或deb-src，描述了文件类型，目录中包含的是二进制软件包（deb），即我们通常使用的已编译好的软件包；或包含的是源码包（deb-src），源码包包含源程序编码、Debian管理文件（.dsc）和“Debian化”该程序所做更改的记录文件diff.gz。

2)/etc/apt/sources.list文件可包含多种类型的地址，APT知道如何处理这些不同的地址类型：http，ftp，file（本地文件，例如：一个加载了ISO9600文件系统的目录）和ssh。

3）更详细的解释可以参见：https://wiki.debian.org/SourcesList#Component


dpkg安装deb包
^^^^^^^^^^^^^^
- dpkg -i package.deb安装包
- dpkg -r package删除包
- dpkg -P package删除包（包括配置文件）
- dpkg -L package列出与该包关联的文件
- dpkg -l package显示该包的版本
- dpkg –unpack package.deb解开 deb 包的内容
- dpkg -S keyword搜索所属的包内容
- dpkg -l列出当前已安装的包
- dpkg -c package.deb列出 deb 包的内容
- dpkg –configure package配置包

dpkg是Debian系统的后台包管理器,类似RPM。也是Debian包管理系统的中流砥柱,负责安全卸载软件包,配置,以及维护已安装的软件包。由于ubuntu和Debian乃一脉相承，所以很多命令是不分彼此的。

Ubuntu中所有packages的信息都在/var/lib/dpkg/目录下,其中子目录”/var/lib/dpkg/info”用于保存各个软件包的配置文件列表.不同后缀名代表不同类型的文件,如:

- .conffiles 记录了软件包的配置文件列表
- .list 保存软件包中的文件列表,用户可以从.list的信息中找到软件包中文件的具体安装位置.
- .md5sums 记录了软件包的md5信息,这个信息是用来进行包验证的.
- .prerm 脚本在Debian报解包之前运行,主要作用是停止作用于即将升级的软件包的服务,直到软件包安装或升级完成.
- .postinst脚本是完成Debian包解开之后的配置工作,通常用于执行所安装软件包相关命令和服务重新启动.

/var/lib/dpkg/available文件的内容是软件包的描述信息,该软件包括当前系统所使用的Debian安装源中的所有软件包,其中包括当前系统中已安装的和未安装的软件包.


make install源代码安装
^^^^^^^^^^^^^^^^^^^^^^^^

apt和dpkg的区别和联系
^^^^^^^^^^^^^^^^^^^^^^^
两者的区别是dpkg绕过apt包管理数据库对软件包进行操作，所以你用dpkg安装过的软件包用apt可以再安装一遍，系统不知道之前安装过了，将会覆盖之前dpkg的安装。

1、dpkg是用来安装.deb文件,但不会解决模块的依赖关系,且不会关心Ubuntu的软件仓库内的软件,可以用于安装本地的deb文件。
    dpkg 使用的是 /var/lib/dpkg/available文件，其内容是软件包的描述信息, 该软件包括当前系统所使用的 Deepin安装源中的所有软件包,其中包括当前系统中已安装的和未安装的软件包.

2、apt会解决和安装模块的依赖问题,并会咨询软件仓库, 但不会安装本地的deb文件, apt是建立在dpkg之上的软件管理工具。
       APT 使用 /var/lib/apt/lists/* 来跟踪可用的软件包，使用apt-get update命令会从/etc/apt/sources.list中下载软件列表，并保存到该目录
