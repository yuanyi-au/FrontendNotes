- [协议模型](#协议模型)
  - [OSI 七层模型](#osi-七层模型)
  - [TCP/IP 五层模型](#tcpip-五层模型)
- [应用层](#应用层)
  - [HTTP/HTTPS](#httphttps)
  - [DNS Domain Name System 域名系统](#dns-domain-name-system-域名系统)
    - [域名的层级结构](#域名的层级结构)
    - [DNS 查找过程](#dns-查找过程)
    - [DNS 劫持](#dns-劫持)
  - [URI（统一资源标识符）和 URL（统一资源定位符）之间的区别](#uri统一资源标识符和-url统一资源定位符之间的区别)
  - [从 url 输入到页面展现发生了什么](#从-url-输入到页面展现发生了什么)
- [传输层](#传输层)
  - [TCP 三次握手 与 四次挥手](#tcp-三次握手-与-四次挥手)
    - [三次握手（ TCP 连接的建立）](#三次握手-tcp-连接的建立)
    - [四次挥手 （TCP 连接的释放）](#四次挥手-tcp-连接的释放)
  - [TCP 协议 与 UDP 协议](#tcp-协议-与-udp-协议)
  - [TCP 传输控制协议](#tcp-传输控制协议)
  - [UDP 用户数据报协议](#udp-用户数据报协议)
- [网络层](#网络层)
- [数据链路层](#数据链路层)
- [网络安全问题](#网络安全问题)
- [正向代理 与 反向代理](#正向代理-与-反向代理)
- [如何判断用户是否登录](#如何判断用户是否登录)
- [跨域](#跨域)
  - [跨域出现的原因](#跨域出现的原因)
  - [跨域的方法](#跨域的方法)
- [单页面路由原理](#单页面路由原理)
- [window.location 和 window.history 在单页面中的应用（改变 URL 页面不刷新）](#windowlocation-和-windowhistory-在单页面中的应用改变-url-页面不刷新)
  - [API](#api)
  - [监听 URL 变化](#监听-url-变化)



# 协议模型

OSI 七层模型 与 TCP/IP 五层模型的区别：OSI 先有模型，后有协议规范，适合于描述各种网络；TCP/IP 是先有协议集然后建立模型，不适用于非 TCP/IP 网络

## OSI 七层模型

从下到上：
1. 物理层：确定与传输媒体的接口的一些特性（EIA/TIA-232, EIA/TIA-499, V.35, 802.3）
2. 数据链路层：链路层协议，检测每一帧的信息（FDDI, Frame Relay, HDLC, SLIP, PPP）
3. 网络层：选择合适的网间路由和交换节点，确保数据按时成功传送（IP, ICMP, ARP, RARP, RIP, IPX）
4. 传输层：为两台主机进程之间的通信提供服务（TCP, UDP）
5. 会话层：负责建立、管理和终止表示层实体之间的通信会话（RPC, SQL, NFS, NetBIOS, names, AppleTalk）
6. 表示层：使通信的应用程序能够解释交换数据的含义（TIFF, GIF, JPEG, PICT）
7. 应用层：通过应用程序间的交互来完成特定的网络应用，需要各种协议（HTTP, TFTP, FTP, NFS, WAIS, SMTP, Telnet, DNS, SNMP）

## TCP/IP 五层模型

从下到上：
1. 物理层
2. 数据链路层
3. 网络层：互联网组管理协议 IGMP，互联网控制报文协议 ICMP
4. 传输层：TCP，UDP
5. 应用层：文件传输协议 FTP，Telnet，DNS，简单邮件传输协议 SMTP 

还有个 TCP/IP 四层模型，将数据链路层和物理层合并

TCP/IP 模型的应用层对应 OSI 模型的上面三层

数据的封装过程：应用层报文 - 传输层报文段 - 网络层 IP 分组 - 链路层添加 MAC 地址封装成数据帧 - 物理层比特流 - 通过传输介质传送到对端


# 应用层

## HTTP/HTTPS

## DNS Domain Name System 域名系统

用于 TCP/IP 网络，提供主机名到 IP 地址的转换服务

DNS 既使用 TCP 又使用 UDP，当进行区域传送（主域名服务器向辅助域名服务器传送变化的那部分数据）时会使用 TCP，当客户端向 DNS 服务器查询域名 ( 域名解析) 时会使用 UDP

### 域名的层级结构

主机名.次级域名.顶级域名.根域名

host.sld.tld.root

### DNS 查找过程

1. 在本地 host 文件、DNS 解析器缓存中查找主机 IP 地址
2. 到本地 DNS 服务器中查询
3. 到根域名服务器中查找顶级域名 IP 地址
4. 逐级向下返回直到找到主机 IP 地址

### DNS 劫持

将原域名对应的 IP 地址进行替换从而使得用户访问到错误的网站或者使得用户无法正常访问网站的一种攻击方式

用户端的一些预防手段：

- 直接通过 IP 地址访问网站，避开 DNS 劫持
- 可以通过网络设置让 DNS 指向正常的域名服务器以实现对目的网址的正常访问，例如将计算机首选 DNS 服务器的地址固定为 8.8.8.8

## URI（统一资源标识符）和 URL（统一资源定位符）之间的区别

两者都定义了资源是什么，URL 是 URI 的一个子集，还定义了如何能访问到该资源

## 从 url 输入到页面展现发生了什么

1. 在浏览器中输入 url 
2. 应用层 DNS 解析域名：先本地查找，再查询 DNS 服务器
3. 应用层客户端发送 HTTP 请求
4. 传输层 TCP 传输报文：三次握手
5. 网络层 IP 协议查询 MAC 地址
6. 数据到达数据链路层
7. 服务器接受数据
8. 服务器响应请求
9. 服务器返回相应文件
10. 浏览器渲染



# 传输层

## TCP 三次握手 与 四次挥手

### 三次握手（ TCP 连接的建立）

目的：为了防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误
1. 客户端向服务器发送一个 SYN 请求报文段，等待服务器确认
2. 服务器端收到 SYN 后，向客服端发送一个 ACK+SYN 报文确认连接请求
3. 客户端收到确认请求后，也向服务器端发送确认报文，然后客户端与服务器端都进入 ESTABLISHED 状态

### 四次挥手 （TCP 连接的释放）

原因：TCP 连接是全双工通信的，单独一方释放连接只代表自己不能再向对方发送数据，需要双方都确认释放连接才可以

1. 客户端向服务器端发送一个 FIN 报文段，申请断开连接，不再发送数据，此时客户端仍可以接收数据
2. 服务器端接收到客户端的请求后，向客户端发送一个确认报文段，表示以后不再接受客户端发过来的数据。但此时还可以向客户端发送数据
3. 服务器端发送完所有数据后向客户端发送 FIN 报文段，申请断开连接，服务器端不再发送数据
4. 客户端接收到请求后，向服务器端发送一个确认应答，进入 TIME_WAIT 阶段，此阶段将持续一段时间（最大生存时间），如果这段时间内服务端没有重发请求，客户端将进入 CLOSED 状态，如果收到重发请求（确认报文丢失）就重发确认报文段，服务器端在收到确认报文后就会进入 CLOSED 状态

## TCP 协议 与 UDP 协议

## TCP 传输控制协议

- 面向连接，无差错、不丢失、不重复、按序到达的可靠交付
- 首部 20 字节
- 只支持一对一通信

## UDP 用户数据报协议

- 无连接，尽最大努力交付（不可靠）
- 首部 8 字节
- 支持一对一，一对多，多对一，多对多通信
- 实时性高，而且没有拥塞控制与流量控制机制，发送速率没有限制




# 网络层





# 数据链路层





# 网络安全问题






# 正向代理 与 反向代理

- 正向代理隐藏了真正的请求客户端，服务端只知道代理服务器是谁
- 反向代理隐藏了真正的服务端，客户端只知道代理服务器是谁，一般用来实现负载平衡



# 如何判断用户是否登录

用户输入的密码由 post 请求发送给服务器，服务器将其与自己数据库中的数据对比，不一致则返回信息给客户端，一致则生成一个 session，同时生成的 session ID 作为 cookie 发送给客户端，对比成功后就重定向至登陆前的页面。判断用户是否登录就是通过这个 session ID（session 过期 -> 登陆过期）






# 跨域

协议、域名、端口任一不同就是跨域的

## 跨域出现的原因

JS 的同源策略：协议+主机号+端口相同才允许访问

在前后端分离的模式下，由于前后端域名不同会产生跨域问题（针对 JS 和 Ajax，HTML 没有跨域问题）

## 跨域的方法

- JSONP：创建一个 <script> 标签，src 指向响应方，只能用 get 方法

- CORS(Cross-origin resource sharing)：允许浏览器向跨源服务器，发出 XMLHttpRequest 请求，克服了 Ajax 只能同源使用的限制。比 JSONP 强大，支持所有类型的 HTTP 请求

- 修改 document.domain 来跨子域：可以把 document.domain 设置成自身或更高一级的父域

- 使用 window.name 跨域：在一个 window 的生命周期内窗口载入的所有页面都是共享一个 window.name 的，每个页面对 window.name 都有读取权限，新页面的载入不会重置 window.name

- 使用 window.postMessage 跨域传送数据

- 服务器代理

- flash

[跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)

# 单页面路由原理

History 模式则是利用了 HTML5 提供的 History API。在 BOM 模型下的 History 对象新增 pushState、replaceState 等 API，允许页面透过脚本操作浏览器的浏览记录而不需要真正的进行页面请求/刷新

![模式流程图](https://img-blog.csdnimg.cn/img_convert/e2c42493141ee3b5c5e28ae346cd1e8d.png)

参考资料：

[JS 前端路由：单页面应用的路由原理和实现](https://blog.csdn.net/weixin_44691608/article/details/115462057?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_title~default-1.control&spm=1001.2101.3001.4242)

# window.location 和 window.history 在单页面中的应用（改变 URL 页面不刷新）

## API

- window.location
  - location.href
  - location.hash

- window.history
  - history.pushState()
  - history.replaceState()
  - history.go()
  - history.back()：相当于 history.go(-1)
  - history.forward()：相当于history.go(1)
  
## 监听 URL 变化

- hashchange：能监听 url hush 的改变
```
window.addEventListener('hashchange', function(e) {
  
})
```
- popstate：能监听除 history.pushState() 和 history.replaceState() 之外的 url 变化
```
window.addEventListener('popstate', function(e) {
  
})
```