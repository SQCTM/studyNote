# DNS域名系统

[TOC]

## 域名的形式

域名是一个有层次的结构，是一串用“.”分隔的多个单词，最右边被称为“顶级域名”，然后是“二级域名”，层级关系向左依次降低，最左边是主机名，通常用来表明主机的用途



## 域名的解析

域名转换成IP地址的过程

- **DNS的核心系统**

  DNS的核心系统是一个三层的树状、分布式服务，基本对应域名的**结构**：

  - **根域名服务器**

    管理顶级域名服务器，返回"com"、"net"、"cn"等顶级域名服务器的IP地址

    *根域名服务器是关键，它必须众所周知，目前全世界共有13组根域名服务器，又有数百台的镜像，保证一定能够被访问到

  - **顶级域名服务器**

    管理各自域名下的权威域名服务器，比如com顶级域名服务器可以返回apple.com域名服务器的IP地址

  - **权威域名服务器**

    管理自己域名下主机的IP地址，比如apple.com权威域名服务器可以返回www.apple.com的IP地址

  *有了这个系统以后任何一个域名都可以在这个树形结构里从顶至下进行查询

- **缓存**

  虽然核心的DNS系统遍布全球，服务能力很强也很稳定，但如果全世界的网民都往这个系统里挤，即使不挤瘫痪了访问速度也会很慢

  在核心DNS系统之外还有两种手段用来减轻域名解析的压力并且能够更快地获取结果，基本思路就是缓存：

  - 许多大公司、网络运营商都会建立自己的DNS服务器，作为用户DNS查询的代理，代替用户访问核心DNS系统，这种服务器被称为**非权威域名服务器**，可以缓存之前的查询结果，如果已经有了记录就无需再向根服务器发起查询，直接返回对应的IP地址

  - **操作系统会对DNS解析结果做缓存**，如果在浏览器里输入以前访问过的网址，就不需要再跑到DNS那里去问了，直接在操作系统里就可以拿到IP地址

    *操作系统中有一个特殊的**主机映射文件**，通常是一个可编辑的文本，如果在操作系统缓存里找不到DNS记录，就会找这个文件

*DNS是一个树状的分布式查询系统，但为了提高查询效率，外围有多级的缓存



## 域名的其他用途

- **重定向**

  因为域名代替了IP地址，所以可以让对外服务的域名不变，而主机的IP地址任意变动，当主机有情况需要下线、迁移时，可以更改DNS记录，让域名指向其他的机器

- **名字服务器**

  因为域名是一个名字空间，所以可以使用开源软件搭建一个在内部使用的DNS，作为名字服务器，这样我们开发的各种内部服务就都用域名来标记，这样发起网络通信时就不必再使用写死的IP地址，可以直接用域名

- **基于域名实现的负载均衡**

  - 因为域名解析可以返回多个IP地址，所以一个域名可以对应多台主机，客户端收到多个IP地址后就可以自己使用轮询算法依次向服务器发起请求，实现负载均衡
  - 域名解析可以配置内部的策略，返回离客户端最近的主机，或者返回当前服务器质量最好的主机，这样在DNS端把请求分发到不同的服务器，实现负载均衡

- **恶意DNS**

  - **域名屏蔽**：对域名直接不解析，返回错误，让用户无法拿到IP地址，也就无法访问网站
  - **域名劫持**：又称域名污染，即你要访问A网站，但DNS给了你B网站

