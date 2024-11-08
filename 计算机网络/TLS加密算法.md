# SSL TLS加密

>SSL Securite Socket Layer 安全套接字层 为Netscape所研发，用以保障在Internet上数据传输之安全，利用数据加密(Encryption)技术，可确保数据在网络上之传输过程中不会被截取。当前版本为3.0。它已被广泛地用于Web浏览器与服务器之间的身份认证和加密数据传输。
>
>TLS Transport Layer Security 传输层安全协议 用于两个应用程序之间提供保密性和数据完整性。它建立在SSL 3.0协议规范之上，是SSL 3.0的后续版本，可以理解为SSL 3.1，它是写入了 RFC的。该协议由两层组成： TLS 记录协议（TLS Record）和 TLS 握手协议（TLS Handshake）。较低的层为 TLS 记录协议，位于某个可靠的传输协议（例如 TCP）上面。
>https = http+ tls

### 对称加密 非对称加密算法 

|                            | 特点                                   | 劣势                                           |
| -------------------------- | -------------------------------------- | ---------------------------------------------- |
| 对称加密算法（DES、AES等） | 加/解密速度快、适合大量数据传输使用    | 1.密码管理麻烦2.密钥传输不安全                 |
| 非对称加密算法(RSA、DH等)  | 公钥加密，私钥解密；私钥加密，公钥解密 | 1.公钥长度长，加密速度慢2.公钥传输的过程不安全 |

# 非对称加密算法 

## 公钥 私钥
公钥（Public Key）和私钥（Private Key）是一对使用特定加密算法产生的密钥对（两串字符串），可以使用 OpenSSL 或者 OpenSSH 的 ssh-keygen 工具生成。公钥和私钥都可以用来加密数据，经过私钥加密的数据，只有通过公钥才可以解密出来，反之亦然。私钥有用户由仔细保管。

## 使用场景
私钥加密，公钥解密。用私钥加密的消息带有**数字签名**，保证消息确实由发布者发布。（若用A的公钥可以解开消息，说明消息确定由A发布）。典型应用场景是，浏览器基于CA根证书（内含CA公钥） 检验网站发来的CA私钥加密的数字证书（包含网站公钥），判断网站身份是否真实。

公钥加密，私钥解密。保证通过公共信道传输消息，保证其他人无法读取消息内容，只有接收者可以解密消息。典型场景是，浏览器端使用网站公钥加密传输 用于随机数，基于该随机数生成后面会话加密数据的对称密钥

### 数字签名

用私钥加密的消息称为**签名**，只有拥有私钥的用户可以生成签名。用公钥解密签名这一步称为**验证签名**，所有用户都可以验证签名(因为公钥是公开的)

#### 1. 生成签名
  一般来说，不直接对消息进行签名，而是对消息的哈希值进行签名，步骤如下。

  1.  对消息进行哈希计算，得到哈希值
  2.  利用私钥对哈希值进行加密，生成**签名**
  3.  将签名附加在消息后面，一起发送过去

#### 2. 验证签名

  1.  收到消息后，提取消息中的签名
  2.  用公钥对签名进行解密，得到哈希值1。
  3.  对消息中的正文进行哈希计算，得到哈希值2。
  4.  比较哈希值1和哈希值2，如果相同，则验证成功。

#### 3. 数字证书 
数字证书是由 Certificate Authority（CA 证书颁发机构）颁发的，所谓数字证书就是 CA 机构使用自己的私钥，对服务器的公钥和一些相关信息（如国家、公司、域名）一起加密的结果，CA 机构在颁发证书前会有一套严格的流程来确保相关信息的准确性。以网站来举例，Github 证书经过 CA 公钥解密后获得的一定就是 Github 的公钥。知名 CA 机构的根证书会内置于游览器中，用来确保 CA 机构本身的身份，其中就包含了 CA 机构自身的公钥。有了证书以后，网站发送信息只要附带上自己的数字证书，就能证明自己的身份。

###  连接建立过程

参考链接：
1.[http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html](http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html)
2.[https://segmentfault.com/a/1190000002554673](https://segmentfault.com/a/1190000002554673)
![](assets/TLS建立过程.png)

* 加密摘要算法 ？ 跟非对称加密什么关系？
  **MD5、SHA**



### x509证书

公钥证书格式

