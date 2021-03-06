# 跨站脚本攻击（XSS）

[TOC]

## XSS攻击

XSS全称Cross Site Scripting，即**跨站脚本**。XSS攻击指**黑客往HTML文件中或者DOM中注入恶意脚本从而在用户浏览页面时利用恶意脚本对用户实施攻击的一种手段**

*最开始这种攻击是通过跨域来实现的，但发展到现在跨域注入脚本已不是唯一的注入手段

- **恶意脚本的危害**

  当页面被注入恶意JS脚本时浏览器无法区分这些脚本是否是被恶意注入的还是正常的页面内容，所以恶意注入JS脚本也拥有所有的脚本权限

  - **窃取Cookie信息**

    恶意JS可以通过"document.cookie"获取Cookie信息，然后通过XMLHttpRequest或者Fetch加上CORS功能将数据发送给恶意服务器

  - **监听用户行为**

    恶意JS可以使用"addEventListener"接口来监听键盘事件，比如可以获取用户输入的信用卡等信息，将其发送到恶意服务器

  - **修改DOM**

    可以通过修改DOM伪造假的登录窗口，用来欺骗用户输入用户名和密码等信息

  - **在页面内生成浮窗广告**

    严重影响用户的体验感



## 恶意脚本的注入方式

- **存储型XSS攻击**
  
  - **步骤**
    1. 黑客利用站点漏洞将一段恶意JS代码提交到网站的数据库中
    2. 用户向网站请求包含了恶意JS脚本的页面
    3. 用户浏览该页面时恶意脚本会将用户的Cookie信息等数据上传到服务器
  
- **反射性XSS攻击**

  反射性攻击中，恶意JS脚本属于用户发送给网站请求的一部分，随后网站又把恶意JS脚本返回给用户，当恶意JS脚本在用户页面中被执行时，黑客就可以利用该脚本做一些恶意操作

  *Web服务器不会存储反射型XSS攻击的恶意脚本

- **基于DOM的XSS攻击**

  不牵涉到页面Web服务器。黑客通过各种手段将恶意脚本注入用户的页面中，比如通过网络劫持在页面传输过程中修改HTML页面的内容，它们会在Web资源传输过程或用户使用页面的过程中修改Web页面的数据



*存储型XSS和反射性XSS攻击都需要经过Web服务器来处理，属于服务器的安全漏洞；基于DOM的XSS攻击全部都是在浏览器端完成，属于前端的安全漏洞



## 阻止XSS攻击

可以通过阻止恶意JS脚本的注入和恶意消息的发送来实现

- **服务器对输入脚本进行过滤或转码**

  可以在服务器端将一些关键的字符进行过滤或转码，这样脚本返回给页面页面就不会执行这段脚本

- **充分利用CSP**

  完全依靠服务器不够，还需要把CSP等策略充分利用起来，以降低XSS攻击带来的风险和后果

  - **CSP功能**
    - 限制加载其它域下的资源文件，即使黑客插入了一个JS文件，这个JS文件也无法被加载
    - 禁止向第三方域提交数据，这样用户数据不会外泄
    - 禁止执行内联脚本和未授权的脚本
    - 提供了上报机制，这样可以帮助我们尽快发现有哪些XSS攻击，以便尽快修复问题

- **使用HttpOnly属性**

  可以通过使用HttpOnly属性来保护Cookie的安全。服务器可以将某些Cookie设置为HttpOnly标志，HttpOnly是服务器通过HTTP响应头来设置的。**使用HttpOnly标记的Cookie只能在HTTP请求过程中使用，无法通过JS来读取**