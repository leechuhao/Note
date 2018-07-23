https://blog.csdn.net/guoweimelon/article/details/50886092

### cookie

cookie机制采用的是在客户端保持状态的方案

网站为了**辨别用户身份**、**进行session跟踪**而储存在**客户端**的数据。（通常经过加密）

由服务端生成，发送给客户端。按照在客户端中储存位置可以分为：内存cookie和硬盘cookie。

内存cookie：

由浏览器维护，保存在内存中，浏览器关闭后就消失了

硬盘cookie

保存在硬盘中，设置生存时间。手动删除和到了生存时间自动删除。

#### 工作原理

1. 创建cookie

- 当用户第一次浏览网站，**服务器**进行以下工作
- 生成唯一的一个识别码（cookie id），创建一个cookie对象；
- 默认是会话级别的cookie，储存在内存中，退出浏览器就会被删除。设置了最大生存周期（maxAge），就会存储在硬盘中。
- cookie放入http响应报头，将cookie插入到Set-Cookie HTTP请求报头中。
- 发送HTTP响应报文。

2. 设置储存cookie

浏览器收到响应报文，会根据Set-Cookie的指示，生成相应的cookie，保存在客户端，该cookie记录着用户当前的信息。

3. 发送cookie

再次访问该网站，会首先检查储存的cookies，如果存在该网站的cookie（即cookie所声明的作用范围大于等于将要请求的资源），则把该cookie放在请求资源的http报文请求头上发送给服务器。

4. 读取cookie

服务器接受到请求报文，从报文头部获取用户的cookie。

####可以实现的功能：

1. 自动登录
2. 购物车功能
3. 记录用户浏览数据，广告推送

#### 缺陷

cookie附加在HTTP请求，增加流量

HTTP中cookie是明文传递 

cookie大小在4KB左右



### session

session代表服务器的一次会话过程，可以是连续的，也可以是时断时续的。

由服务器生成，保存在服务器的内存、缓存、硬盘或数据库中。

session机制采用的是在服务器端保持状态的方案。

#### 工作原理

1. 创建session

当用户访问到一个服务器，如果服务器启用了session。服务器就要为用户创建 一个session。

在创建session的时候，首先检查用户发来的请求报文汇总是否包含了一个session id，如果有则说明该用户之前访问过服务器，就会根据这个session id将在服务器内存中的session找出来（找不到就会重新创建一个），如果请求报文中没有这个session id，则为该客户端创建一个session并生成一个对应的session id。

session id是唯一的，不重复的，不容易找到规律的字符串。

session id会被保存在本次响应中，返回客户端保存，保存方式正是cookie。

2. 使用session

如果禁止了cookie，session id仍然可以继续使用，方法为：**URL重写**和**表单隐藏字段**

URL重写：把session id直接附加在URL路径后面表现形式为：

```
http://…./xxx;jSession=ByOK3vjFD75aPnrF7C2HmdnV6QZcEbzWoWiBYEnLerjQ99zWpBng!-145788764； 
```

或者作为查询字符串附加在URL后面

```
http://…../xxx?jSession=ByOK3vjFD75aPnrF7C2HmdnV6QZcEbzWoWiBYEnLerjQ99zWpBng!-145788764 
```

表单隐藏字段：

服务器自动修改表单，添加一个隐藏字段，在提交表单时能把session id传会服务器。

#### 功能

判断用户是否曾经登录过

购物车功能

### 二者区别

1. 存放位置不同

cookie放在客户端，session放在服务端

2. 存取方式

cookie只能保管ASCII字符串，假如需要存取Unicode字符或者二进制数据，需要先进行编码。cookie也不能直接存取java对象

session可以存取任何类型的数据，能直接保管java bean，可以把session看作是一个java容器类。

3. 安全性（隐私策略）

cookie存储在浏览器中，对用户可见，一些程序可能会窥探用户的cookie

session放在服务器中，对用户透明

使用建议：敏感的信息如帐号密码等不要写在cookie中，最好是先进行加密，提交到服务器之后在进行解密。

4. 有效期

可以将cookie的生存时间设置为一个很大的数字，cookie就会在浏览器保存很长时间。由于session依赖于session id这一cookie，而该数据过期时间默认为-1，即关闭浏览器（一次会话结束）就会失效

5. 对服务器的压力

session保存在服务器中，每个用户都会生成一个session，假如用户达到一定数量就会耗费大量的内存

cookie保存在客户端，不占用服务器资源。

6. 跨域支持

cookie支持跨域名访问，假如将domain属性设置为“baidu.com”，则以“baidu.com”为后缀的一切域名都可以访问cookie