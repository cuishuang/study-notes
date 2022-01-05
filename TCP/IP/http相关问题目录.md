---
title: http相关问题目录
date: 2017-03-25 20:36:23
tags: TCP/IP
---

<br>


### <font color="SpringGreen">保持长连接的字段(key=>value)</font>

<br>

**connection:keep-alive**

带这个字段,TCP连接就可以复用了;一个http处理完之后,另外一个http数据直接继续用这个连接。减少新建(三次握手)和断开TCP(四次挥手)连接的消耗。

HTTP/1.1之后默认开启Keep-Alive, 在HTTP的header中增加Connection选项。当设置为Connection:keep-alive表示开启，设置为Connection:close表示关闭




http中的<font color="red">keep-alive</font>和TCP中的<font color="red">KeepAlive</font>有所差异

参考: [KeepAlive相关](https://www.jianshu.com/p/9fe2c140fa52)



<br>

---

<br>

### <font color="DarkTurquoise">Cookie中与安全相关的机制</font>

<br>


Cookie的字段一般分为:

[name(键)] [value(值)] [path(路径)] [domain(所属域)] [expires(过期时间)] [secure flag(安全标志)] [httponly flag]

<br>

**其中name=value是必选项，其它都是可选项**

- name: 一个唯一确定的cookie名称。通常来讲cookie的名称是不区分大小写的。

- value: 存储在cookie中的字符串值。最好为cookie的name和value进行url编码

- path: 表示这个cookie影响到的路径，浏览器跟会根据这项配置，像指定域中匹配的路径发送cookie。

- domain: cookie对于哪个域是有效的。所有向该域发送的请求中都会包含这个cookie信息。这个值可以包含子域(如：[http://yq.aliyun.com]()，也可以不包含它(如：[http://aliyun.com]()，则对于[http://aliyun.com]() 的所有子域都有效).

- expires: 失效时间，表示cookie何时应该被删除的时间戳(也就是，何时应该停止向服务器发送这个cookie)。如果不设置这个时间戳，浏览器会在页面关闭时即将删除所有cookie；不过也可以自己设置删除时间。这个值是GMT时间格式，如果客户端和服务器端时间不一致，使用expires就会存在偏差。

- max-age: 与expires作用相同，用来告诉浏览器此cookie多久过期（单位是秒），而不是一个固定的时间点。正常情况下，max-age的优先级高于expires。

- Secure flag , 安全标志，指定后，只有在使用SSL链接时候才能发送到服务器，如果是http链接则不会传递该信息。就算设置了secure 属性也并不代表他人不能看到你机器本地保存的 cookie 信息，所以不要把重要信息放cookie就对了服务器端设置

- HttpOnly flag，HttpOnly: 告知浏览器不允许通过脚本document.cookie去更改这个值，同样这个值在document.cookie中也不可见。但在http请求张仍然会携带这个cookie。注意这个值虽然在脚本中不可获取，但仍然在浏览器安装目录中以文件形式存在。这项设置通常在服务器端设置。



---

<br>

####  <font color="DoderBlue">Secure Cookie机制</font>

<br>


`Secure Cookie`机制指的是设置了secure标志的cookie。

Secure Cookie仅在https层面上安全传输，如果是http请求，就不会带上cookie。

这样能降低重要的cookie被中间人截获的风险。
不过，也不是说可以万无一失。因为secure cookie对于客户端脚本来说是可读可写的，可读就意味着secure cookie能被盗取，可写意味着能被篡改，所以还是存在一定的风险。

<br>

---

<br>


#### <font color="DoderBlue">HttpOnly属性</font>

<br>


Cookie的HttpOnly属性，指浏览器不要在除HTTP/HTTPS请求之外暴露Cookie。

一个有HttpOnly属性的Cookie，不能通过非HTTP/HTTPS方式来访问，例如通过调用JavaScript(如引用document.cookie），能尽可能减少黑客使用跨域脚本攻击（XSS）来"偷走"Cookie。(但仍存在被xss攻击泄露Cookie的可能)。

<br>


---

<br>

#### <font color="DoderBlue">Same-Site属性</font>

<br>


当用户从`http://a.com`发起`http://b.com`的请求也会携带上Cookie，从`http://a.com`携带过来的Cookie称为`第三方Cookie`。

为了防止CSRF（Cross-site request forgrey,跨站点请求伪造）攻击，可以使用SameSite属性。

```javascript
Set-Cookie: CookieName=CookieValue; SameSite=Lax; 

Set-Cookie: CookieName=CookieValue; SameSite=Strict;
```

strict模式下：浏览器在任何跨域请求中都不会携带Cookie，这样可以有效的防御CSRF攻击，但是对于有多个子域名的网站采用主域名存储用户登录信息的场景，每个子域名都需要用户重新登录，造成用户体验非常的差。




<br>

---

<br>


#### <font color="DoderBlue">本地cookie与内存cookie</font>

<br>


本地cookie与内存cookie，区别在于cookie设置的`expires`字段。

**如果没有设置过期时间,就是内存cookie, 随着浏览器的关闭而从内存中消失**

如果设置了过期时间是未来的某一个时间点，那这个cookie就会以文本的形式保存在操作系统本地，待过期时间到了才会消失。

很多网站为了提升用户的体验，省去每次用户登陆的麻烦，采用本地cookie的方式，让用户可以在未来的一个月，或者半年，永久等时间段内不需要进行登陆操作。

这也意味着，用户被攻击的风险变大了。攻击者通过xss得到这样的本地cookie后，能够在未来很长一段时间内，甚至是永久控制这目标用户的账号权限。

然而，即使采用内存cookie也存在一定的风险，攻击者可以给内存cookie加一个过期时间，使其变成本地cookie，这样还是无法避免以上的安全问题。说到底，用户账号是否安全与服务器端校验有关，包括重要cookie的唯一性（是否可预测），完整性（是否被篡改），过期等校验。


<br>

参考:

[Web安全：你必须知道的“Cookie安全”](https://zhuanlan.zhihu.com/p/65558789)

[浅谈cookie安全](https://zhuanlan.zhihu.com/p/58666986)



