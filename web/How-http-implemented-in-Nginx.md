# HTTP 协议在 Nginx 中是如何实现的？

先从 Nginx 官网扒一下官方(www.nginx.org) 的定义：

> nginx [engine x] is an HTTP and reverse proxy server, a mail proxy server,...

从中可以看到，Nginx 最重要的定位：首先它是一个 HTTP 服务器，其次它还可以作为反向代理服务器使用。而使用 Nginx 的大部分用户主要也就是为了使用它的这个功能。

这篇文章，我们将着重研究 Nginx 中的 HTTP 服务器功能是如何实现的。会从理论以及 Nginx 代码实现两方面进行讨论，接下来，我们开始正文部分。

## HTTP 协议

HTTP 全称为：`Hypertext Transfer Protocol`, 从名字可以看出它本质上是一个协议，相当于一个约定，大家按照协议中约定的数据格式、方法来交换数据，以此达到通信的目的。目前应用最广的协议版本为 HTTP 1.1，该版本经典的请求消息格式如下所示：

```text
GET /index.html HTTP/1.1
Host: www.zhihu.com
user-agent: google/chrome
accept: */*
```

在收到客户端的请求消息之后，HTTP 服务器需要根据请求的内容，回复合适的响应消息，消息格式如下：

```text
HTTP/1.1 200 OK
content-type: text/html; charset=utf-8
content-length: 2345
server: nginx/1.17.6
cache-control: no-cache
etag: "63172035-16e2"

<!doctype html><head><title>Zhi Hu</title></head><body>Hello, World</body></html>
```

这两个消息格式很重要，HTTP 协议的实现主要就是围绕它们展开的。从上述消息格式可以看到，每一个 HTTP 消息均由三个部分组成：元信息、头部字段、数据，其中元信息部分主要记录 HTTP 版本号、请求方法、请求资源的路径、响应状态码等 HTTP 规范中规定的信息，一般来说是不能写入随意的信息；而头部字段是 HTTP 协议中客户端、服务端双方交流最主要的 “语言”，还能够加入以 `X-` 为前缀的自定义头部字段来实现说 “黑话” 的效果；而数据部分就是该条消息所包含的要传输的内容了。

HTTP 服务器最主要的功能，就是将客户端发送的 TCP 报文中所包含的 HTTP 消息体提取出来，转义为可以被服务端程序使用的数据结构，经过一番处理过后，得到响应消息的数据结构，再将这些数据结构以字符串的形式写到一个 TCP 报文中，返回给客户端。接下来我们看看在 Nginx 中是如何实现整个处理流程的。

## Nginx 中的实现

Nginx 进程在启动的时候，


## 自己实现一个

知道了一个大概的实现过程，我们接下来依葫芦画瓢，用 Go 语言，自己实现一个最简版本的 HTTP 服务器。


## 总结

HTTP 协议是应用最广的应用层协议之一，成就了如今繁荣的互联网产业。

这篇文章我们主要讨论了 HTTP 协议在 Nginx 中是如何实现的这一话题，弄明白这一点对于我们更好的做 Web 层面的开发工作非常重要。知其然更要知其所以然。

最后，我们还用 Go 语言，自己写了一个简单的 HTTP 服务器，通过去除所有对边缘情况的处理细节，只保留处理 HTTP 请求及响应的核心逻辑，能够更好的帮助读者理解实现 HTTP 协议的核心逻辑。

我们知道，HTTP 消息需要依赖 TCP 协议在两个设备之间传送，而 TCP 协议是在操作系统内核层面实现的。关注我，下一篇文章，我们一起看看 TCP 协议在 Linux 内核里面是如何实现的。