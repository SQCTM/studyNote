# HTTP的实体数据

[TOC]

## 数据类型与编码

HTTP主要应用两种数据类型标准规范，**MIME type**和**Encoding type**，这样无论是浏览器还是服务器就都可以识别出body的类型，就能够正确处理数据

- **MIME type** 

  即多用途互联网邮件扩展，将数据分成了八大类，每个大类下再细分出多个子类，形式是 "**type/subtype**" 的字符串，以下是HTTP经常遇到的几个类别：

  - **text**

    即文本格式的可读数据，最熟悉的就是text/html ，表示超文本文档，此外还有纯文本text/plain、样式表 text/css 等

  - **image**

    即图像文件，有 image/gif、image/jpeg、image/png 等

  - **audio/video**

    音频和视频数据，例如 audio/mpeg、video/mp4 等

  - **application**

    数据格式不固定，可能是文本也可能是二进制，必须由上层应用程序来解释

    常见的有application/json，application/javascript、application/pdf 等，如果实在是不知道数据是什么类型，就会是application/octet-stream，即不透明的二进制数据

  *仅有MIME type还不够，因为HTTP在传输时为了节约带宽，有时会**压缩数据**，因此产生了Encoding type

- **Encoding type**

  告诉了浏览器数据是用的什么编码格式，浏览器能正确解压缩，还原出原始的数据

  Encoding type常用的只有三种：

  - **gzip**：GNU zip 压缩格式，是互联网上最流行的压缩格式
  - **deflate**：zlib（deflate）压缩格式，流行程度仅次于gzip
  - **br**：一种专门为HTTP优化的新压缩算法（Brotli）



## 数据类型使用的头字段

HTTP协议为MIME type和Encoding type定义了两个**Accept请求头字段**和两个**Content实体头字段**，用于客户端和服务端进行“**内容协商**”

- **使用对象**

  - 客户端用**Accept**头告诉服务端希望收到什么样的数据
  - 服务端用**Content**头告诉客户端实际发送了什么样的数据

- **数据类型字段**

  - **Accept字段**

    标记的是客户端可以理解的**MIME type**，可以用 ”,“ 做分隔符列出多个类型，让服务器有更多选择的余地，如：`Accept: text/html,application/xml,image/webp,image/png`

  - **Content-Type字段**

    服务器在响应报文里用头字段Content-Type告诉实体数据的真实类型，如：

    `Content-Type: text/html
    Content-Type: image/png`

- **压缩格式字段**

  - **Accept-Encoding字段**

    标记的是客户端支持的压缩格式，可以用 ”,“ 列出多个让服务器选择其中一种来压缩数据，如：

    `Accept-Encoding: gzip, deflate, br`

  - **Content-Encoding字段**

    服务器实际使用的压缩格式放在响应头字段Content-Encoding里，如：

    `Content-Encoding: gzip`

  - 这两个字段可以省略。如果请求报文中没有Accept-Encoding字段就表示客户端不支持压缩数据；如果响应报文中没有Content-Encoding字段就表示响应数据没有被压缩



## 语言类型与字符集的头字段

- **语言类型**

  解决国际化的问题，使用的是 "**type-subtype**" 形式的字符串。例如，en表示任意的英语，en-US表示美式英语，en-GB表示英式英语，zh-CN表示我们最常使用的汉语

- **字符集**

  处理文字的字符编码，**Unicode**和**UTF-8**把世界上所有的语言都容纳在一种编码方案里，**UTF-8成为了互联网上的标准字符集**

- **语言类型头字段**

  - **Accept-Language**

    标记了客户端可以理解的自然语言，可以用 “,” 做分隔符列出多个类型，如：

    `Accept-Language: zh-CN, zh, en`

  - **Content-Language**

    服务器在响应报文里用该字段告诉客户端实体数据使用的实际语言类型，如：

    `Content-Language: zh-CN`

- **字符集头字段**

  - **Accept-Charset**

    标记了客户端支持的字符集，如：`Accept-Charset: gbk, utf-8`

  - **Content-Type**

    响应头中没有对应的Content-Charset，而是**在Content-Type字段的数据类型后面用“charset=xxx”来表示**，如：`Content-Type: text/html; charset=utf-8`



## 内容协商的质量值

客户端用 Accept、Accept-Encoding、Accept-Language 等请求头字段进行内容协商的时候，还**可以用“q”参数表示权重来设定优先级**

- **权重值**

  权重的最大值是1，最小值是0.01，默认值是1，如果值是0就表示拒绝

- **表示形式**

  在数据类型或语言代码后面加一个“;”，然后是“q=value”，如：

  `Accept: text/html,application/xml;q=0.9,*/*;q=0.8`

  例子中它表示浏览器最希望使用的是 HTML 文件，权重是 1；其次是 XML 文件，权重是 0.9；最后是任意数据类型，权重是 0.8。服务器收到请求头后会计算权重，再根据自己的实际情况优先输出HTML或者XML

  *在HTTP 的内容协商里 “;” 的意义小于 “,” 



## 内容协商的结果

内容协商的过程是不透明的，每个Web服务器使用的算法都不一样。有时服务器会在响应头中加一个**Vary**字段，**记录服务器在内容协商时参考的请求头字段**，如：

`Vary: Accept-Encoding,User-Agent,Accept`

这个Vary字段表示服务器依据了Accept-Encoding、User-Agent和Accept这三个头字段，决定了发回的响应报文

*Vary字段可以认为是响应报文的一个特殊的“版本标记”。当Accept等请求头变化时，Vary也会随着响应报文一起变化。因此同一个URI可能会有多个不同的“版本”，这主要用在传输链路中间的代理服务器实现缓存服务