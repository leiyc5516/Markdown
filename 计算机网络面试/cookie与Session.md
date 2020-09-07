cookie:

　　1.定义:什么是cookie? 　cookie就是存储在客户端的一小段文本

　　2.cookie是一门客户端的技术,因为cookie是存储在客户端浏览器中的

　　3.cookie的作用:是为了实现客户端与服务器之间状态的保持

　　4.cookie 技术不安全,不要使用cookie保存敏感信息 

　　5.cookie默认 在浏览器关闭之后,就立即实现失效.如果想指定cookie的过期时间,需要通过使用expires属性实现.在服务器响应返回响应头时

　　　　写入cookie的过期时间. 即响应头设置 set-cookie:[expires=new.Date(Date.now() +10 *1000)]  10S后过期

原理:由于http协议是无状态的.传统服务器只能被动响应请求.当服务器获取到请求,并为了能够区分每一个客户端,需要客户端发送请求时发送一个标识符(cookie),

也因此为了提供这个标识符,产生了cookie技术.我们在请求头(Request Headers)中添加了标识符(cookie). 每次发送请求,都会把这个cookie随同其它报文一起发送给服务器.

服务器根据报文中cookie,进行区分客户端浏览器.

　　如何设置表示符:

　　　　在node中可以在writeHeaer的时候通过Set-Cookie来将表示通过响应报文发送给客户端 , 或客户端通过插件 jquery.cookie

session:

　　　　由于http无状态,服务器在每次连接中持续保存客户端的私有数据,此时需要结合cookie技术,通过session会话机制,在服务器端保存每一个http请求的私有数据

原理:

　　　　在服务器内存开辟一块内存空间,专门存放每个客户端私有数据,每个客户端根据cookie中保存的私有sessionId,可以获取到独属于自己的session数据.　　　

session在node中使用:

　　　　

1. 安装session模块

   ```bash
   npm install express-session -S
   ```

2. 导入session模块

   ```js
   var session = require('express-session')
   ```

3. 在express中使用

   ```
   session
   ```

   中间件：

   ```js
   // 启用 session 中间件
   app.use(session({
   secret: 'keyboard cat', // 相当于是一个加密密钥，值可以是任意字符串
   resave: false, // 强制session保存到session store中
   saveUninitialized: false // 强制没有“初始化”的session保存到storage中
   }))
   ```

4. 将私有数据保存到当前请求的session会话中：

   ```js
   // 将登录的用户保存到session中
   req.session.user = result.dataValues;
   // 设置是否登录为true
   req.session.islogin = true;
   ```

5. 通过

   ```
   destroy()
   ```

   方法清空

   ```
   session
   ```

   数据：

   ```js
   req.session.destroy(function(err){
   if(err) throw err;
   console.log('用户退出成功！');
   // 实现服务器端的跳转，这个对比于 客户端跳转
   res.redirect('/');
   });
   ```

 