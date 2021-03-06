## 网络相关

[TCP/IP - 百度百科](https://baike.baidu.com/item/TCP/IP%E5%8D%8F%E8%AE%AE)

[HTTP协议详解](http://www.cnblogs.com/ranyonsue/p/5984001.html)

#### http状态码

**12345法则**
- 1** 消息
100  客户端应当继续发送请求。
- 2** 成功
200 成功
- 3** 重定向
301 永久重定向，例如http定向到https
302 临时重定向，例如js跳转
- 4** 请求错误
403 forbidden 拒绝请求。
404 not found 找不到请求的网页。
- 5** 服务器错误
500 Internal Server Error 服务器内部错误，例如php代码错误

#### http版本

##### 1.0

- 无状态、无连接。
HTTP1.0规定浏览器和服务器保持短暂的连接，浏览器的每次请求都需要与服务器建立一个TCP连接，服务器处理完成后立即断开TCP连接（无连接），服务器不跟踪每个客户端也不记录过去的请求（无状态）。

- 队头阻塞
HTTP1.0规定下一个请求必须在前一个请求响应到达之前才能发送。假设前一个请求响应一直不到达，那么下一个请求就不发送，同样的后面的请求也给阻塞了。

##### 1.1

- 长连接
HTTP1.1增加了一个Connection字段，通过设置Keep-Alive可以保持HTTP连接不断开，避免了每次客户端与服务器请求都要重复建立释放建立TCP连接，提高了网络的利用率。如果客户端想关闭HTTP连接，可以在请求头中携带Connection: false来告知服务器关闭请求。

- 管道化
基于HTTP1.1的长连接，使得请求管线化成为可能。管线化使得请求能够“并行”传输。

- 新的字段
如cache-control，支持断点传输，以及增加了Host字段（使得一个服务器能够用来创建多个Web站点）。

##### 2.0

- 二进制分帧
HTTP2.0通过在应用层和传输层之间增加一个二进制分帧层，突破了HTTP1.1的性能限制、改进传输性能。

- 多路复用（连接共享）
HTTP2.0实现了真正的并行传输，它能够在一个TCP上进行任意数量HTTP请求。而这个强大的功能则是基于“二进制分帧”的特性。

- 头部压缩
HTTP2.0使用encoder来减少需要传输的header大小，通讯双方各自cache一份header fields表，既避免了重复header的传输，又减小了需要传输的大小。高效的压缩算法可以很大的压缩header，减少发送包的数量从而降低延迟。

- 服务器推送
服务器除了对最初请求的响应外，服务器还可以额外的向客户端推送资源，而无需客户端明确的请求。

#### http和https的区别
https是在http的基础上加ssl层。进行了数据加密。保证传输过程中，数据加密，默认端口是443
http在传输中数据是明文 ，默认端口80
||http|https|
-|-|-
端口|80|443
内容|明文传输|加密传输
安全|无状态|需要安全证书

HTTPS 约等于 HTTP+SSL
- 优点
相对安全/SEO排名更高
- 缺点
证书需要申请，服务器资源占用更高，连接建立需要传送证书，速度更慢.

#### TCP/IP

[四层模型及OSI七层参考模型](https://blog.csdn.net/guoguo527/article/details/52078962)

[三次握手四次挥手](https://www.cnblogs.com/Jessy/p/3535612.html)

简略快速回忆版：
- 三次握手
客户端：我要和你通信(syn-sent)
服务端：你的请求已收到，发送确认(syn-rcvd)
客户端：你的确认已收到，连接建立(est)

- 四次挥手
客户端：我没有东西了，准备关闭(fin-wait)
服务端：你的关闭我收到了，但我还有点东西没传完(close-wait)
……一段时间后……
服务端：我的东西传完了，可以关闭了(last-ack)
客户端：收到关闭通知，你也可以关闭了(time-wait)

