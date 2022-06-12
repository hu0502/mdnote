### JWT

#### 1. 定义

>JWT 全称 Json Web token，是为了在网络应用环境间传递声明而执行的一种基于Json的开放标准（RFC 7519），该token被设计为紧凑且安全的，特别适用于分布式站点的单点登录（SSO）场景。JWT的声明一般被用来在身份提供者和服务提供者之间传递被认证的用户身份信息，以便于从服务器获取资源，也可以增加一些额外的其他业务逻辑所必须的声明信息，该token也可之间被用于认证，也可以被加密。 


#### 2. 使用场景

授权认证，一旦用户登录，后续每个请求都将包含JWT，系统在每次处理用户请求前都要先进行JWT安全校验，通过之后再进行处理。



#### 3. 组成

由三部分组成，用.拼接

+ Header

+ PayLoad

+ Signature，创建签名, 使用编码后的header和payload以及一个密钥, 使用header中指定的签名算法进行签名, 生成的签名作为JWT的第三部分, 该签名是在服务端完成的, 客户端不知道密钥, 更安全

  

#### 4. 安装

PHP安装JWT

````shell
composer require firebase/php-jwt
````

#### 5. 其它

+  "Cannot handle token prior to 2021-11-01T17:38:03+0800"

  ````php
   JWTUtil::$leeway = 60;
  ````

  





