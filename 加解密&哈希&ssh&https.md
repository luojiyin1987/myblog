# 加解密  哈希   SSH    HTTPS
## Encryption 和 Hash（哈希）的区别
1. 从信息论角度

    -  Encryption 是可逆的，没有信息熵的改变。
    -  Hash是不可逆，把东西打散，一般会导致信息熵减少


2. 应用角度

   - Encryption 常用来做为密钥的加解密（AES, RSA, ECC）
   - Hash 常被用于做数字签名， 数据校验（CRC, SHA, MD5）


3. 形象比喻
   - Encryption 就是带密码的保险箱
   - Hash就榨汁机，有去无回

## 加密算法分为对此(Symmetric)  非对称(Asymmetry)两大类
  + 对称加密核心在于加密、解密的密钥是一样的

     - 凯撒密码
     - AES(Advanced Encryption Standard)
     - SM1 SM4
     - DES、3DES、 RC4等

  + 非对称(Asymmetry)加密
  加密，解密的密钥分为两组，而且不能相互反推。这种算法在现实生活中很难类比。大致就是通过某算法生成一对密钥对k1， k2 ，用k1加密之后的密文只能用k2解密，用k2加密的密文只能用k1解密
   - 基于因数分解的算法

     RSA、DSA是其中的代表，Linux的SSH就是基于这两种算法进行文件 key auth。由于GPU和超级计算机的算力提升，好多Linux发行版默认是2048位

  - 椭圆曲线算法（Elliptic Curve Cryptography  ECC）
     可以用1/6的密钥长度达到RSA更高的强度。不太懂数学

  非对称加密算法非常安全，但是慢。RSA的加密速度是AES的1/30。 就出现了SSL TLS。

  ## HTTP + TLS = HTTPS
  TLS 的前身是SSL
   - SSL(Secure Socket Layer, 安全套接字层)，为Netscape研发。SSL协议位于TCP/IP协议与各应用层协议之间。
   - TLS跟SSL 是有差异的。我还是不太理解具体差异。

   做个比喻 A与B通信， A是SSL客户端，B是SSL服务端，加密的消息放在[] 里，以突出明文消息的区别，双方的处理的动作放在()里说明  

   A： 我想和你安全通话，我这里的对称算法有DES RC5 密钥交换算法有RSA和DH，摘要算法有MD5 和SHA

   B： 我们用DES－RSA－SHA 这对组合好了。
   这是我的证书，里面有我的姓名和公钥，你拿去验证一下我的身份（把证书发给A）

   A：(查看证书上B的姓名是否无效，并通过手头上已有的CA证书验证了B证书的真实性，如果其中一项有误，就发出警告断开连接，这一步保证了B公钥的真实性)
   （产生一份秘密消息，这份秘密消息将用做加密密钥，加密初始化向量（IV）和 hmac的密钥。将这份秘密消息－协议称为  per_master_secert 用B的公钥加密，封装成ClientKeyExchange的消息，由于用B的公钥，保证第三方无法窃听）
   我生成一份秘密消息，并用了你的公钥进行加密，给你（ ClientKeyExchange发给B）
   注意 下面我将用加密的办法给你发信息！
   （将秘密进行处理，生成加密密钥， 加密初始化向量和hmac的密钥）
   ［我说完了］

   B:(用自己的私钥将ClientKeyExchange中的秘密消息解密出来，然后将秘密消息进行处理，生成加密密钥，初始化加密向量和hmac的密钥，这时双方已经安全协商出一套加密办法了)
   注意，我也要开始用加密的办法给你发消息了！                                  
   ［我说完了］

   A:[我的秘密是。。。。。。。]

   B:[其他人是听不到的]

   上个图

   ![TLS流程图](http://o7q7atccj.bkt.clouddn.com/tls-ssl.svg)
   TODO： 理解HTTPS更多背后的原理。知识的广度是知识的深度的副产品



## ssh

SSH 是Secure Shell 的缩写，是建立在应用层的和传输层基础上的安全协议。维计算机运行的Shell提供安全的传输和使用环境。

   传统的rsh、FTP、POP、Telnet协议在传输过程中采用明文，很容易受到中间人攻击。最初SSH协议是由芬兰的一家公司于1995年设计开发的，受版权和加密算法的限制，很多人转向OpenSSH。
待续。。。
