(web)API
=============

key&secret key
-----------------
- key和secret key应该是一个意思，就是密钥，即“秘密信息”。可能只包括一个值，也可能是多个值的合集。
- key和secret key的概念在"加解密"、"完整性验证"以及"用户认证"三个场景中都会用到。

Authentication
-------------------
Authentication就是认证用户身份，一个api不能随便就提供访问，key简称API接口验证序号，是用于验证API接入合法性的。接入哪个网站的API接口，就需要这个网站允许才能够接入。

API可以支持多种“用户认证”的方式，每个方式中涉及的核心概念的名字不同，但内涵是相同的。
https://swagger.io/docs/specification/authentication/ 这个链接解释了6种不同的“api认证请求用户”的方式。

- onvideo api看上去使用了"Cookie Authentication"

appid, apikey, secretKey
-----------------------------

百度平台
^^^^^^^^^^
1. 在百度云平台上创建一个应用时，会提供三个一套的参数，appid, apikey, secretKey。

- appid是针对在云平台上创建的应用，
- 而apikey和secretkey是成对出现的，是云平台针对API的；
- 一个应用可以访问很多的api，可以用一套apikey+secretkey访问所有接口。

2. 同一个 app_id 可以对应多个 apikey+secretkey, 这样 平台就可以分配你不一样的权限, 比如 apikey1 + secretkey 只有只读权限 但是 apikey2+secretkey 有读写权限。这样你就可以把对应的权限 放给不同的开发者。

有道平台
^^^^^^^^^^

- 和每个应用对应的两个：“应用ID”（就是appkey）和“应用密钥”，并无api key。
- appkey和密钥仅仅对请求进行“签名”，并没有对请求内容进行加密，见请求参数http://ai.youdao.com/docs/doc-trans-api.s#p04：sign, 签名信息，sha256(appKey+input+salt+curtime+密钥)


科大讯飞
^^^^^^^^^^^^^^

- 科大讯飞的验证方法是，加一个http header，X-CheckSum=MD5(APIKey + X-CurTime + X-Param)，后台拿到这个checksum后，后台把它存储的apikey也做同样的计算，如果相等，就验证通过。
- 但是在讯飞云平台中，在我创建的应用中，申请了多个服务，每个服务的APIKey都是一样的。

refresh token
---------------
参考链接 https://juejin.im/post/5a56f7f75188257340262691#heading-0

.. image:: ../_images/refresh_token.jpg