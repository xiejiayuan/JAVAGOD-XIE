# HTTP基础知识

它是一种**无状态**的超文本传输协议（英文：**H**yper**T**ext **T**ransfer **P**rotocol，缩写：HTTP）

HTTP 是基于 TCP/IP 协议的[**应用层协议**](https://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html)。主要规定了客户端和服务器之间的通信格式，默认使用80端口。

最早版本是1991年发布的0.9版。该版本极其简单，只有一个命令`GET`。

> ```http
> GET /index.html
> ```



## 一、HTTP1.0

1996年发布，首先，任何格式的内容都可以发送。

其次，除了`GET`命令，还引入了`POST`命令和`HEAD`命令，丰富了浏览器与服务器的互动手段。

再次，HTTP请求和回应的格式也变了。除了数据部分，每次通信都必须包括头信息（HTTP header），用来描述一些元数据。

 

### 请求格式与响应格式

- 请求行
- 请求首部
- 报文实体



![image-20210731184236821](http://note.youdao.com/yws/public/resource/7d81e6a39024a96dd86efacf29f4ca80/xmlnote/WEBRESOURCEa5e4fe83c15641bfaf53bc233d3c2505/1979)

### 缺点

**无连接**

每个TCP连接只能发送一个请求。

发送数据完毕，连接就关闭，如果还要请求其他资源，就必须再新建一个连接。

比如浏览一个包含多张图片的HTTP页面，则会产生大量的通信开销

#### 怎么办？

持久连接



### HTTP的五大特点

1. 支持**客户/服务器**模式。
2. **简单快速**：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有`GET`、`HEAD`、`POST`。每种方法规定了客户与服务器联系的类型不同。由于`HTTP`协议简单，使得`HTTP`服务器的程序规模小，因而通信速度很快。
3. **灵活**：HTTP允许传输任意类型的数据对象。正在传输的类型由`Content-Type`加以标记。
4. **无连接**：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。早期这么做的原因是请求资源少，追求快。
5. **无状态**：`HTTP`协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。



## 二、HTTP1.1

1997年1月，HTTP/1.1 版本发布，只比 1.0 版本晚了半年。它进一步完善了 HTTP 协议，一直用到了20年后的今天，直到现在还是最流行的版本。

### 变化

#### （1）持久连接（核心变化）

只要任意一端**没有明确**提出断开连接，则保持TCP连接状态。



#### （2）管道机制

即在同一个TCP连接里面，客户端可以同时发送多个请求。这样就进一步改进了HTTP协议的效率。

可以并行发多个请求，不用等待响应就可以发下一个。





### Cookie

既然http是无状态的，那我怎么保证用户的状态呢？

比如登录状态，购物车清单。

#### 使用Cookie技术就可以

Cookie技术通过在请求和响应报文中写入Cookie信息来控制客户端的状态。

#### 过程

客户端第一次请求也没有Cookie，服务端进行响应时，返回一个带有Set-Cookie的首部字段信息的响应，

通知客户端保存Cookie，下次客户端再请求的时候，就把Cookie带上一起发送。

这时候，服务端接受到Cookie后，就去检测是哪个客户端发来的请求，然后对比服务端的记录，最后得到之前的状态信息。

![image-20210731183620577](http://note.youdao.com/yws/public/resource/7d81e6a39024a96dd86efacf29f4ca80/xmlnote/WEBRESOURCE61a8e700a88048fd9dcccbbf0e2a263f/1978)

### HTTP的缺点

- 使用明文通信（未加密），内容可能会**被窃听**
- 不验证通信方的身份，因此可能**遭遇伪装**
- 无法证明报文的完整性，所以有可能已**遭篡改**





## 三、HTTPS

**就是身披SSL外壳的HTTP**

HTTP加上**加密处理和认证以及完整性保护**后即是HTTPS

![image-20210731193414762](http://note.youdao.com/yws/public/resource/7d81e6a39024a96dd86efacf29f4ca80/xmlnote/WEBRESOURCE7d5287919ff44f779c1985a16a7e3fb1/1980)

![image-20210731194600182](http://note.youdao.com/yws/public/resource/7d81e6a39024a96dd86efacf29f4ca80/xmlnote/WEBRESOURCEf5f90cff271141d4b76ab80667481a10/1981)



### 工作流程

- 1、TCP 三次同步握手
- 2、客户端验证服务器数字证书
- 3、DH 算法协商对称加密算法的密钥、hash 算法的密钥
- 4、SSL 安全加密隧道协商完成
- 5、网页以加密的方式传输，用协商的对称加密算法和密钥加密，保证数据机密性；用协商的hash算法进行数据完整性保护，保证数据不被篡改。



### 传输过程

总结：

1. 客户端发起HTTPS请求，将自己支持的**一套加密规则**发送给网站。 **（协商加密算法）**
2. 服务端返回公开秘钥证书（非对称秘钥）、对称加密算法种类等信息。**（协商加密算法）**
3. 客户端验证证书的有效性，若有效，本地生成一个**对称加密算法的随机数（共享秘钥）**，并通过服务端公钥，传输过去。
4. 服务器接收到公钥，通过自己的私钥解密，拿到共享秘钥，以及报文校验码秘钥。
5. 以后就通过共享秘钥进行通信。



非对称加密算法（公开密钥）：用来传输共享秘钥，RSA

对称加密算法（共享秘钥）：用来加密传输内容,AES

HASH算法：用来校验内容，MD5

![img](http://note.youdao.com/yws/public/resource/7d81e6a39024a96dd86efacf29f4ca80/xmlnote/WEBRESOURCEf52d06cfd29340f4af348d37bd5dd73a/1994)

### 3.1 加密方式

（1）共享秘钥

加密解密都是一个秘钥

优点：简单，效率高。

缺点：秘钥可能被窃取。



（2）公开秘钥

两把秘钥，一把公开秘钥，一把私有秘钥

**用对方公开秘钥加密，用自己私有秘钥解密**

优点：很安全。

缺点：很笨重，效率低

![image-20210731221130476](http://note.youdao.com/yws/public/resource/7d81e6a39024a96dd86efacf29f4ca80/xmlnote/WEBRESOURCE693a09b64fab46d3bb2997ca02eb6251/1982)



![image-20210731221140130](http://note.youdao.com/yws/public/resource/7d81e6a39024a96dd86efacf29f4ca80/xmlnote/WEBRESOURCE4201e2daa4234aa2a7bcadcd225afb09/1983)

### 3.2 HTTPS使用混合加密机制

为了既要保证安全，又要传输高效，则使用混合加密

在交换秘钥环节使用公开秘钥加密，加密的东西就是后面共享秘钥的秘钥。

之后的建立通信交换报文阶段，则使用共享秘钥加密。



核心：先用**笨重的锁**，**运输灵活的钥匙**，后面再用**灵活的锁通信。**



### 3.3 证明公开密钥正确性的证书

即时使用了公开秘钥，锁住共享秘钥秘钥，但还是有问题？
公开秘钥被调包了怎么办？（禁止套娃）

#### 用数字证书

花钱买证书，找第三方给你担保，

来保证公开密钥的真实性、可靠性。（第三方担保）

数字证书有可靠的第三方机构颁发。

![image-20210801095611531](http://note.youdao.com/yws/public/resource/7d81e6a39024a96dd86efacf29f4ca80/xmlnote/WEBRESOURCE71357d9c97e74e8fa108ef4314921f52/1984)

#### 服务端可以买证书，我客户怎么办？

有些认证机构可以颁发客户端证书，但仅用于特殊用途的业务，比如说银行转账

但只能说证明客户端的真实存在，但不能证明是不是客户本人。



#### 为什么说非对称加密慢

因为对称加密主要的运算是位运算，速度非常的快。

AES算法，本质上来说就是位移和替换。

而非对称加密计算一般都比较复杂，如RSA，涉及大量的大数乘法，大数模运算。



## 四、HTTP与HTTPS的区别



- **安全性**

  HTTP是明文传输，不加密，HTTPS是加密传输，安全性高

- **费用**

  HTTPS需要申请证书，是需要花钱买的。

- **性能**

  HTTP响应速度快，TCP三次握手，只需要交换3个包就行了

  HTTPS响应速度慢，除了TCP的3个包，还需要SSL握手的9个包，共有12包

  HTTP简单、灵活，开销小

  HTTPS建立在SSL之上，所以更消耗服务器CPU和内存资源，能处理的请求书也会减少。

- **端口不同**

  HTTP是80端口

  HTTPS是443端口



**HTTPS并不能取代HTTP**

HTTPS开销较大，如果不涉及个人隐私数据等敏感数据，就用HTTP协议就行了。



## 五、 GET与POST

GET 和 POST 其实都是 HTTP 的请求方法

| 请求方法 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| GET      | 请求指定的页面信息，并返回实体主体。**（读，获取服务器资源）** |
| POST     | 向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。**（写，修改服务器资源）** |





### 5.1 GET特点

1. 用来**读**服务器的资源的，用来获取服务器资源。
2. 参数是可见的，请求的参数会附在 URL 之后（放在请求行中），以 ? 分割 URL 和传输数据，多个参数用 & 连接。

2. 它是安全和幂等的（写不会造成安全问题，幂等就是说每次请求的结果都是一样的）
3. 请求是会浏览器主动缓存的，下次同样的请求就直接去缓存里拿
4. URL会有长度限制，不可能无限长
5. 只发送一个TCP数据包



### 5.2 POST特点

1. 用来写服务器的资源的，一般用来提交表单数据。
2. 参数不可见，请求的参数会放在请求实体里。
3. 不会被缓存。
4. 发送两个TCP数据包，第一次先将请求头发给服务器，待服务器响应100 Continue，浏览器再发送请求数据，服务器响应200 OK。



### 5.3 区别

1. 参数携带位置不同

2. 缓存

   GET 可以被缓存，POST没有缓存。

3. 安全性

   POST更安全些，因为参数不会被保存在浏览器历史或 web 服务器日志中。

4. 对数据长度的限制

   GET有，POST没有，因为POST的参数是放在请求实体里的。

本质没有区别，POST的URL 也可以带有参数。

只是便于管理。



## 六、 HTTP首部字段

### 6.1 通用字段

````
Connection		//连接管理,管理持久连接：Keep-Alive，close
Cache-Control	//控制缓存行为
Date			//创建报文的时间
````



### 6.2  实体首部字段

````
Content-Encoding	//实体主体适用的编码方式
Content-Language	//实体主体的自然语言
Content-Length		//实体主体的大小（单位：字节）
Content-Type		//实体主体的媒体类型
Expires				//资源过期的日期时间
Last-Modified		//资源的最后修改日期时间
````

  

### 6.3  请求首部字段

````
Host			//服务器的主机名：多个虚拟主机可能在一个IP上，用HOST表明是哪个主机
User-Agent		//浏览器的名称
Accept			//用户可处理的媒体类型
Accept-Charset 	//优先的字符集：unicode,iso
Accept-Encoding	//优先的内容编码：gzip
Accept-Language	//优先的语言（自然语言，中文，英文）：zh-cn
````



### 6.4 响应首部字段

````
Server			//服务器安装的应用程序信息
Age				//创建响应过去的时间，单位为秒
Location		//重定向的URL（如果要重定向的话）
````



## 七、消除HTTP瓶颈SPDY

### 7.1 什么瓶颈

- 单向请求
- 首部未压缩
- 冗余首部



### 7.2 技术

**1）AJAX异步加载**

实现了WEB页面的局部刷新



**2）Comet延迟响应**

通过延迟响应，模拟实现服务器向客户端的推送功能



**3）WebSocket全双工通信**

通信的过程中可以相互主动发数据。

通信时，不再使用HTTP的数据帧，而是使用WebSocket的数据帧



## 七、HTTP 2.0

### 7.1 目标

提供用户使用WEB的**速度体验**



### 7.2 HTTP1.1 瓶颈

**1）TCP连接数限制**

为了实现高并发请求，不得不开多个TCP连接，但是多开的数量是有限制的。

虽说可以用**域名分片**技术突破数量限制，但是每个连接还是很多额外消耗。



**2）头部阻塞问题**

每个TCP连接同时只能处理一个请求-响应，按照先进先出原则，一个响应没有返回，后面的请求-响应都受阻。

虽说提出了管线化技术，但是一个响应慢，还是会阻塞后续响应。因为多个请求是按序的，多个响应也是安序的。



**3）报文头臃肿，且内容变化不大**



**4）明文传输不安全**





### 7.3 HTTP2变化及优势

**1） 二进制分帧**层

**在HTTP/2中，在应用层（HTTP2.0）和传输层（TCP或者UDP）之间加了一层：二进制分帧层。**

**在二进制分帧层中， HTTP/2 会将所有传输的信息分割为互不依赖的更小的帧（frame）,并对它们采用二进制格式的编码。**

帧是数据传输的最小单位，用二进制传输代替原本的明文文本传输，是其它功能和性能优化的基础。



**2）多路复用解决线头阻塞问题**

**在一个HTTP的连接上，多路“HTTP消息”同时工作**。



http1.1为了实现高并发请求，不得不使用多开tcp连接的方式

HTTP2对于同一域名的通信，**都是在一个连接上进行完成。**

HTTP2把每个消息划分成了为**互不依赖的帧**，不同流的帧之间可以**交错发送**，因为可以按照帧首部的流ID进行组装，**多个消息交织在了一起，大大提高了效率**。

解决了HTTP1.1的按需接收导致的线头阻塞问题。



实现：

1. 切割：把一个HTTP消息切割成多个更小的互不依赖的帧
2. 标识：每个帧都带有流ID，标识自己属于哪个流
3. 交错发送



**3）服务器推送**（贴心）

浏览器发送一个请求，服务器主动向浏览器推送与这个请求相关的资源，这样浏览器就不用发起后续请求。



**4）报文头压缩（HPACK）**

多个不同的请求的头部可能变化不大， 每次都完整携带，太浪费资源，于是引入了头部压缩。



**5）请求优先级**

高优先级的流都应该优先发送，但又不会绝对的
