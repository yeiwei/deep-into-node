## Crypto

### 什么是 SSL ？
Secure Sockets Layer，这是其全名，他的作用是协议，定义了用来对网络发出的数据进行加密的格式和规则。

```js
      +------+                                            +------+
服务器 | data | -- SSL 加密 --> 发送 --> 接收 -- SSL 解密 -- | data | 客户端 
      +------+                                            +------+   

      +------+                                            +------+
服务器 | data | -- SSL 解密 --> 接收 <-- 发送 -- SSL 加密 -- | data | 客户端 
      +------+                                            +------+
```
> 注：TLS 1.0 等同于 SSL 3.1，TLS 1.1 等同于 SSL 3.2，TLS 1.2 等同于 SSL 3.3。



### OpenSSL
OpenSSL 是在程序上对 SSL 标准的一个实现，提供了：

* libcrypto 通用加密库
* libssl TLS/SSL 的实现
* openssl 命令行工具

Node.js 是完全采用 OpenSSL 进行加密的，其相关的 TLS HTTPS 服务器模块和 Crypto 加密模块都是通过 C++ 在底层调用 OpenSSL 。

OpenSSL 实现了对称加密:

AES(128) | DES(64) | Blowfish(64) | CAST(64) | IDEA(64) | RC2(64) | RC5(64)

非对称加密:

DH | RSA | DSA | EC

以及一些信息摘要:
MD2 | MD5 | MDC2 | SHA | RIPEMD | DSS

其中信息摘要是一些采用哈希算法的加密方式，也意味着这种加密是单向的，不能反向解密。这种方式的加密大多是用于保护安全口令，比如登录密码。这里面我们最长用的是 MD5 和 SHA (建议采用更稳定的 SHA1, MD5通过查表大法已经不再单向)。


### TLS HTTPS 服务器
想要建立一个 Node.js TLS 服务器，需要使用 tls 模块:

`var Tls = require('tls');`

在开始搭建服务器之前，我们还有些重要的工作要做，那就是证书，签名证书。

基于 SSL 加密的服务器，在与客户端开始建立连接时，会发送一个签名证书。客户端在自己的内部存储了一些公认的权威证书认证机构，即 CA。客户端通过在自己的 CA 表中查找，来匹配服务器发送的证书上的签名机构，以此来判断面对的服务器是不是一个可信的服务器。

如果这个服务器发送的证书，上面的签名机构不在客户端的 CA 列表中，那么这个服务器很有可能是伪造的，你应该听说过“中间人攻击”。

```js
+------------+                                
| 真正的服务器 |              选择权:连接？不连接？  +-------+
+------------+             +------------------ | 客户端 |
                    https  |                   +-------+
+--------------+           | 拦截通信包       
| 破坏者的服务器 | ----------+
+--------------+  证书是伪造的
```

### 总结




### 参考
* http://www.jianshu.com/p/a8b87e436ac7