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
   - SSL(Secure Socket Layer, 安全套接字层)，为Netscape研发。SSL协议位于
