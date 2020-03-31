（web)Session&Cookie
========================
Cookie
---------
.. image:: ../_images/Cookie_Session001.png

由上图可得，

1. cookie使用两个http header扩展了http。

- server端响应中带的Set-Cookie头
- client端请求中带的Cookie头

Session
----------
- Session由服务器发起，服务器向客户端浏览器发送一个名为JSESSIONID的Cookie，它的值为该Session的id