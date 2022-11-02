# 从源码层面来看 Nginx 是如何处理 HTTP 请求的！

先从 Nginx 官网扒一下官方(www.nginx.org) 的定义：

> nginx [engine x] is an HTTP and reverse proxy server, a mail proxy server,...

从中可以看到，Nginx 最重要的定位：首先它是一个 HTTP 服务器，其次它还可以作为反向代理服务器使用。而使用 Nginx 的大部分用户主要也就是为了使用它的这个功能。

这篇文章，我们将着重研究 Nginx 中的 HTTP 服务器功能是如何实现的。会从理论以及 Nginx 代码实现两方面进行讨论，接下来，我们开始正文部分。

## HTTP 协议

HTTP 全称为：`Hypertext Transfer Protocol`, 从名字可以看出它本质上是一个协议，相当于一个约定，大家按照协议中约定的数据格式、方法来交换数据，以此达到通信的目的。目前应用最广的协议版本为 HTTP 1.1，该版本的消息格式如下所示：

```text
GET /create.html HTTP/1.1
host: www.zhihu.com
content-length: 2345
vary: Accept-Encoding
content-type: text/html; charset=utf-8
server: nginx/1.17.6

body line 1
body line 2
```

