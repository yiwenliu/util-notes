SSH
====
Refer
------
阮一峰写了两篇文章，很值得参考

`SSH原理与运用（一）：远程登录 <http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html>`_

`SSH原理与运用（二）：远程操作与端口转发 <http://www.ruanyifeng.com/blog/2011/12/ssh_port_forwarding.html>`_

SSH的公开密钥认证方式
----------------------
1. 用户将自己的公钥储存在远程主机上。
2. 登录的时候，远程主机会向用户发送一段随机字符串，用户用自己的私钥加密后，再发回来。远程主机用事先储存的公钥进行解密，如果成功，就证明用户是可信的，直接允许登录shell，不再要求密码。

密钥生成
----------
使用命令 **ssh-keygen** 生成、管理和转换认证密钥，`中文手册 <http://www.jinbuguo.com/openssh/ssh-keygen.html>`_

使用场景
---------
- 链接Github上已有仓库时进行认证