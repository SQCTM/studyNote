# HTTP的连接管理

[TOC]

## 短连接

HTTP底层的数据传输基于TCP/IP，每次发送请求前需要先与服务器建立连接，收到响应报文后会立即关闭连接，因为客户端和服务器的整个连接过程很短暂，不会与服务器保持长时间的连接状态，所以被称为“短连接”

- **缺点**

  TCP建立连接和关闭连接都是非常”昂贵“的操作。建立连接要有“三次握手“，发送3个数据包，需要1个RTT；关闭连接是”四次挥手“，4个数据包，需要2个RTT。而HTTP的一次简单”请求-响应“通常只需要4个包，如果不算服务器内部的处理时间，最多是2个RTT，**传输效率非常之低**

 

## 长连接

又称持久连接、连接保活、连接复用。即把这个时间成本由原来的一个“请求-应答”均摊到多个“请求-应答”上

- **缺点**

  TCP连接长时间不关闭，服务器必须在内存里保存它的状态，这占用了服务器的资源。如果有大量的空闲长连接只连不发，会很快耗尽服务器的资源，导致服务器无法为真正有需要的用户提供服务



## 相关连接的头文字

- **请求头中**

  - 因为长连接对性能的改善效果非常显著，所以在HTTP/1.1中的连接都会默认启用长连接，只要向服务器发送了第一次请求，后续的请求都会重复利用第一次打开的TCP连接，也就是长连接，在这个连接上收发数据
  - 也可以在请求头里明确地要求使用长连接机制，使用的字段是**Connection**，值是**keep-alive**

- **响应报文中**

  不管客户端是否显式要求长连接，如果服务器支持长连接，它就会在响应报文中放一个**Connection:keep-alive**

- **关闭长连接**

  长连接需要在恰当的时间关闭，不能永远保持与服务器的连接

  - 在客户端可以在请求头中加上**Connection:close**字段，告诉服务器此次通信后就关闭连接，服务器收到后在响应报文中也加上这个字段，发送后就调用Socket API关闭TCP连接
  - 服务器通常不会主动关闭连接，但也可以使用一些策略



## 队头阻塞

队头阻塞与短连接和长连接无关，而是由HTTP基本的“请求-应答”模型所导致的

因为HTTP规定报文必须是“一发一收”，这就形成了一个先进先出的串行队列，队列中的请求没有优先级，只有入队的先后顺序，排在最前面的请求被最优先处理，如果前面的请求因为处理太慢耽误时间，则队列里后面的请求也不得不跟着一起等待，导致其他请求承担了不应有的时间成本



## 性能优化

- **并发连接**

  可以使用**并发连接**解决队头阻塞问题，即同时**对一个域名发起多个长连接**

  - **缺点**

    客户端为了速度建立过多连接，服务器的资源扛不住，或者被服务器认为是恶意攻击，会造成“拒绝服务”

- **域名分片**

  多开几个域名，这些域名都指向同一台服务器，这样实际长连接的数量就又上去了