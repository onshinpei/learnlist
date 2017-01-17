# 构建有多个房间的聊天程序
### 将要创建的聊天程序需要完成如下任务
* 提供静态文件(HTML、CSS和客户端JS)
* 在服务端上处理与聊天相关的信息
* 在用户的浏览器中处理与聊天相关的信息

为了提供静态文件，需要使用Node内置的htp模块。但是通过HTTP提供文件时，通常不能只是发送文件中的内容，还应该有所发文件的类型。也就是说要用正确的MIME类型设置HTTP头的Content-Type。为了查找MIME类型，要用到第三方模块mime

> [MIME类型](http://baike.baidu.com/link?url=8FqFpGo3TeGw-gVnI8ZebuKsThGPgWIMMlG7Nu52wkG7TokboXNK4I-nowLfJknvKIMdQfIm0VNMp_EE3jjVVK) 多用途互联网邮件扩展类型

为了处理聊天相关的信息，需要Ajax轮询服务器。但是为了让这个程序尽可能快地做出响应，我们不会用传统的Ajax发送消息。Ajax用HTTP作为传输机制，并且HTTP本来就不是做实时通讯的。在用HTTP发送消息时，必须用一个新的TCP/IP链接。打开和关闭链接需要的时间。此外因为每次请求都要发送HTTP请求头，所以传输的数据量比较大。这个程序没用依赖于HTTP的方案。而是采用了[WebSocket](http://baike.baidu.com/link?url=s1GWmqhQLOUcJjVdSL7IpYJKQmxuT98aeExWvlbYvl98wBDh_COmBSvgfehyhdjfO-Tb2pIKZ-gWiSVDDlwpfWWAnlEEa0_cpgpeS630n2S),这是一种为支持实时通讯而设计的轻量的双向通信协议。因为大多数情况下，只有兼容HTML5的浏览器才支WebSocket，所以这个程序会使用流行的[Socket.IO](http://socket.io)库，它给不能使用WebSocket的浏览器提供了一些后备措施，包括使用Flash。Socket.IO对后备功能处理是透明的，不需要额外的代码和配置。
### 接下来我们准备来实践这个项目
项目的package.json文件,需要安装socket.io和mine两个模块
>npm install socket.io mime --save
``` javascript
{
  "name": "charts",
  "version": "1.0.0",
  "description": "Minimalist multiroom chat server",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "wxp",
  "license": "ISC",
  "dependencies": {
    "mime": "^1.3.4",
    "socket.io": "^1.7.2"
  }
}
```
