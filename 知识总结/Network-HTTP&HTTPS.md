- [HTTP Hypertext Transfer Protocol 超文本传输协议](#http-hypertext-transfer-protocol-超文本传输协议)
  - [HTTP 报文](#http-报文)
    - [HTTP 请求报文](#http-请求报文)
    - [HTTP 响应报文](#http-响应报文)
  - [HTTP 常见状态码](#http-常见状态码)
  - [HTTP 方法](#http-方法)
  - [Keep-Alive 和非 Keep-Alive 对服务器性能的影响](#keep-alive-和非-keep-alive-对服务器性能的影响)
  - [HTTP 版本](#http-版本)
    - [HTTP 1.0](#http-10)
    - [HTTP 1.1](#http-11)
    - [HTTP 2.0](#http-20)
    - [HTTP 3.0](#http-30)
  - [Cookie、session、token](#cookiesessiontoken)
- [HTTPS  Hyper Text Transfer Protocol over SecureSocket Layer 超文本传输安全协议](#https--hyper-text-transfer-protocol-over-securesocket-layer-超文本传输安全协议)
  - [Socket](#socket)
  - [Websocket](#websocket)
  - [http 和 websocket 的长连接区别](#http-和-websocket-的长连接区别)
- [HTTP 与 HTTPS 对比](#http-与-https-对比)

# HTTP Hypertext Transfer Protocol 超文本传输协议 

组成：
- 起始行：报文描述
- 头部：报文属性
- 主体：报文数据

有两种连接模式，持续连接与非持续连接，持续链接下 TCP 默认不关闭，可以避免每次建立连接三次握手所花费的时间，非持续连接每次请求对象都需要建立一个全新的连接

## HTTP 报文

### HTTP 请求报文

HTTP 请求报文包括请求行、请求头、空行、请求体四部分

- 请求行：包括方法字段、URL 字段 和 HTTP 协议版本字段三部分
	- 方法字段：GET、POST、HEAD、PUT、DELETE、OPTIONS、TRACE、CONNECT
		- GET：获取数据，请求的数据放在请求行中，所以可以在 URL 中看到
		- POST：向服务器发送数据，提交的数据放在请求体中，所以 URL 中看不到
    *注意：GET 和 POST 字段的长度限制是浏览器的限制，不是 HTTP 协议的限制*
		
- 请求头：键值对，典型的请求头有：
	- User-Agent：产生请求的浏览器类型
	- Host：请求的主机名，允许多个域名同处一个IP 地址，即虚拟主机;
　　- Accept：客户端可识别的响应内容类型列表，\* 用于按范围将类型分组，用 \*/\* 指示可接受全部类型，用 type/\* 指示可接受 type 类型的所有子类型
　　- Accept-Language：客户端可接受的自然语言
　　- Accept-Encoding：客户端可接受的编码压缩格式
　　- Accept-Charset：可接受的应答的字符集
　　- connection：连接方式(close 或 keepalive)
　　- Cookie：存储于客户端扩展字段，向同一域名的服务端发送属于该域的cookie

- 请求体：在 GET 方法中请求体为空，在 POST 方法中请求体为提交的数据

### HTTP 响应报文

HTTP 响应报文包括状态行、响应头、空行、响应体四部分

- 状态行：包括协议版本、状态码、状态码描述三部分

- 响应头：键值对
	- Server：与 User-Agent 相对应，一个发送客户端软件（浏览器）的信息，一个发送服务端软件的信息
	- ETag: 某一个固定的 URI 资源发生变化时，ETag 会更新
	- Content-Length：文档长度
	- Content-type：媒体类型
	- Connection：连接方式(close 或 keepalive)
	- Location：重定向位置
	
- 响应体：返回给客户端的信息

参考资料：

[HTTP报文结构](https://blog.csdn.net/u012813201/article/details/70207726)

[HTTP请求报文和响应报文详解](http://www.yanghuiqing.com/network/371)

## HTTP 常见状态码

1xx表示信息响应
- 100	Continue，已收到请求，客户端应继续

2xx表示成功响应

- 200 OK，表示从客户端发来的请求在服务器端被正确处理
- 204	No Content，请求已处理，无返回，客户端不更新视图
- 205	Reset Content，请求已处理，无返回，客户端应更新视图

3xx表示重定向
- 301 Moved Permanently，永久重定向，默认可缓存，搜索引擎应更新链接
- 302 Found，临时重定向，默认不缓存，除非显示指定
- 304 Not Modified，未修改，不含响应体
- 307	Temporary Redirect，临时重定向，默认不缓存，除非显示指定，不改变请求方法和请求体
- 308	Permanent Redirect，永久重定向，默认可缓存，搜索引擎应更新链接，不改变请求方法和请求体

4xx表示客户端错误
- 400	Bad Request，请求语义或参数有误，不应重复请求
- 401 Unauthorized，请求需身份验证或验证失败
- 404 Not Found，未找到，无原因
- 408	Request Timeout，请求超时
- 410	Gone，资源已被永久移除
- 415	Unsupported Media Type，请求文件类型服务端不支持

5xx表示服务端错误
- 500 Internal Server Error，服务端报错，通常是脚本错误
- 502	Bad Gateway，网关无响应，通常是服务端环境配置错误
- 503 Service Unavailable，服务端临时不可用，建议返回Retry-After，搜索引擎爬虫应一段时间再次访问这个URL
- 504	Gateway Timeout，网关超时，通常是服务端过载
- 511	Network Authentication Required	需要身份验证，常见于公用 WIFI

## HTTP 方法

- HTTP 1.0 ：GET，POST，HEAD 
- HTTP 1.1 ：OPTIONS，PUT, PATCH，DELETE，TRACE，CONNECT

## Keep-Alive 和非 Keep-Alive 对服务器性能的影响

非 Keep-Alive 来说，必须为每一个请求的对象建立和维护一个全新的连接，每一个连接都需要客户机和服务器分配 TCP 的缓冲区和变量，会给服务器带来的严重的负担，在 Keep-Alive 方式下，服务器在响应后保持该 TCP 连接打开，在同一个客户机与服务器之间的后续请求和响应报文可通过相同的连接进行传送。不过长时间保持 TCP 连接容易导致系统资源被无效占用，也会造成损失，重点是要恰当 Keep-Alive 模式配置

## HTTP 版本

### HTTP 1.0

无状态，无连接的应用层协议

- 无法复用连接
每次发送请求，都要新建连接

- 队头阻塞
下个请求必须在上个请求响应到达后发送。如果上个请求响应丢失，则后面请求被阻塞

### HTTP 1.1

HTTP1.1 继承了 HTTP1.0 简单，克服了 HTTP1.0 性能上的问题

- 长连接
新增Connection: keep-alive保持永久连接

- 管道化
支持管道化请求，请求可以并行传输，但响应顺序应与请求顺序相同。实际场景中，浏览器采用建立多个TCP会话的方式，实现真正的并行，通过域名限制最大会话数量

- 缓存处理
新增Cache-control，支持强缓存和协商缓存

- 断点续传

- 主机头
新增Host字段，使得一个服务器创建多个站点

### HTTP 2.0

HTTP2.0进一步改善了传输性能

- 二进制分帧
在应用层和传输层间增加二进制分帧层

- 多路复用
建立双向字节流，帧头部包含所属流 ID，帧可以乱序发送，数据流可设优先级和依赖，实现并行传输

- 头部压缩
压缩算法编码原来纯文本发送的请求头，通讯双方各自缓存一份头部元数据表，避免传输重复头

- 服务器推送
服务端可主动向客户端推送资源，无需客户端请求

### HTTP 3.0

当一个 TCP 会丢包时，整个会话都要等待重传，后面数据都被阻塞。这是由于 TCP 本身的局限性导致的。HTTP3.0 基于 UDP 协议，解决 TCP 的局限性

- 0-RTT
缓存当前会话上下文，下次恢复会话时，只需要将之前缓存传递给服务器，验证通过，即可传输数据

- 多路复用
一个会话的多个流间不存在依赖，丢包只需要重发包，不需要重传整个连接

- 更好的移动端表现
移动端 IP 经常变化，影响 TCP 传输，HTTP3.0 通过 ID 识别连接，只要 ID 不变，就能快速连接

- 加密认证的根文
TCP 协议头没有加密和认证，HTTP3.0 的包中几乎所有报文都要经过认证，主体经过加密，有效防窃听，注入和篡改

- 向前纠错机制
每个包还包含其他数据包的数据，少量丢包可通过其他包的冗余数据直接组装而无需重传。数据发送上限降低，但有效减少了丢包重传所需时间

## Cookie、session、token

HTTP 协议是无状态协议，数据交换完毕后就会关闭客户端与服务器端的连接，请求与请求之间不保存状态，意味着服务器无法跟踪会话

-|cookie|session|token
存取位置|客户端|服务器端|数据库
存取值类型|字符串|大多数类型|加密字符串
大小|受客户端限制|自行配置|
有效时间|写入时设置，用户可清除|自行配置，服务器端定时清除过期 session 防止内存溢出|自行配置，过期后需重新验证
跨域|可设置跨子域，不可跨主域|可跨域跨服务器|多平台跨域

# HTTPS  Hyper Text Transfer Protocol over SecureSocket Layer 超文本传输安全协议

三次握手，使用 SSL/TLS 协议进行加密传输

- TSL 传输安全层协议：对称加密和非对称加密结合，数字证书

- SSL 安全套接字协议：对称算法，公用密钥算法

## Socket

Socket是应用层与 TCP/IP 协议族通信的中间软件抽象层，它是一组接口。在设计模式中，Socket 其实就是一个门面模式，它把复杂的 TCP/IP 协议族隐藏在 Socket 接口后面，对用户来说，一组简单的接口就是全部，让 Socket 去组织数据，以符合指定的协议

## Websocket

Websocket是html5提出的一个协议规范，是为解决客户端与服务端实时通信。本质上是一个基于tcp，先通过HTTP/HTTPS协议发起一条特殊的http请求进行握手后创建一个用于交换数据的TCP连接

WebSocket优势： 浏览器和服务器只需要要做一个握手的动作，在建立连接之后，双方可以在任意时刻，相互推送信息。同时，服务器与客户端之间交换的头信息很小

## http 和 websocket 的长连接区别

HTTP1.1 通过使用 Connection:keep-alive 进行长连接，HTTP 1.1 默认进行持久连接。在一次 TCP 连接中可以完成多个 HTTP 请求，但是对每个请求仍然要单独发 header，Keep-Alive 不会永久保持连接，它有一个保持时间，可以在不同的服务器软件（如Apache）中设定这个时间。这种长连接是一种“伪链接”

websocket 的长连接，是一个真的全双工。长连接第一次 tcp 链路建立之后，后续数据可以双方都进行发送，不需要发送请求头

keep-alive 双方并没有建立正真的连接会话，服务端可以在任何一次请求完成后关闭。WebSocket 它本身就规定了是真正的、双工的长连接，两边都必须要维持连接的状态

# HTTP 与 HTTPS 对比

项目|HTTP|HTTPS
--|--|--
默认端口|80|443
HTTP 版本|1.0 - 1.1|2 - 3
传输|明文，易被劫持|加密，不易劫持
CA 认证|不支持|支持
SEO|无优待|优先
页面响应速度|快，只需要三次握手|较慢，需要三次握手+SSL协商过程