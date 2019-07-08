

## Q1: 什么是 Node.js?

Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。它是一个开源和跨平台的服务端应用程序。任何人都可以编写 JavaScript 代码来开发 Node.js 应用程序。它可以运行于 Microsoft Windows,Linux, or OS 系统。

Node.js 不是一个新的语言，也不仅仅是一个基于 JavaScript 的框架，它基于 Chrome 的 JavaScript 运行时，因此代码的编写和执行与浏览器非常相似。

**Node.js 功能**

以下是 Node.js 的一些重要功能

- **高度可扩展**

Node.js 使用的单线程模型且采用了事件循环架构，使得编写可扩展性高的服务器变得既容易又安全。一些传统的服务端语言会创建多线程来处理请求，通常创建线程都是有系统资源开销的，因此也会有一些限制，而 Node.js 只创建一个线程来处理更多的请求。

- **事件驱动和异步**

Node.js 的所有 API 都是异步的。这意味着下一个请求来临时可以直接处理而不用等待上一次的请求结果先返回。

- **No Buffering**

Node.js 从不缓冲任何任何数据，参见[What is No-Buffering feature of Node.js](https://stackoverflow.com/questions/42596571/what-is-no-buffering-feature-of-node-js)

我们许多人可能会对 Node.js 感到困惑。它不是像 Apache 这样的 Web 服务器。Node.js 提供了一种新方法来执行我们的代码。它是 JavaScript 的运行时。Node.js 提供了创建 HTTP 服务器的方法，我们可以在这之上托管我们的应用程序。

Source: [Introduction To Node.js](https://www.c-sharpcorner.com/article/introduction-to-node-js/)

## Q2: 如何安装 Node.js？

我们可以从 Node.js 官方网站 [https://nodejs.org/en/](https://nodejs.org/en/) 下载安装软件。

**nvm 安装**

这里推荐您使用 nvm 工具安装，方便后期的升级进行 Node.js 版本管理，一下为安装步骤：

* 安装 nvm：wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
* 查看所有 Node.js 版本：nvm ls-remote
* 查看本地 Node.js 版本：nvm ls
* 安装 Node.js：nvm install v6.9.5
* 设置系统的默认 Node.js 版本：nvm alias default v6.9.5

**验证安装结果**

在 Node.js 安装成功之后，我们可以检查它是否正常工作。

打开命令终端，输入以下命令

```node
$ node
```

之后将出现 Node 提示符，我们写入以下命令，运行查看

```js
console.log("hello world!");   
```

按 Enter 键

![](./img/node-interview/install-node-check.jpg)


## Q3: 如何用 Node.js 监听 80 端口？

这是有陷阱的，在类似一些 Unix 系统中你不应该尝试监听 80 端口，这么做你需要拥有超级用户权限，因此，不推荐你这么做。

尽管如此，如果你一定要让应用监听 80 端口，可以使用 Nginx 来实现，在应用前方加上一层反向代理。还是建议你监听大于 1024 的端口。

## Q4: 什么是错误优先的回调函数？

错误优先回调函数用于同时返回错误（error）和数据信息（data），返回值的第一个参数做为错误信息描述，并且验证它是否出错（非错 error 为 null），其它参数用于返回数据。

```js
fs.readFile(filePath, function(err, data) {  
  if (err) {
    // 错误信息处理
    return console.log(err)
  }
  
  // return the data object
  return data;
})
```

## Q5: 你可以在 Node.js 中创建 Http 服务吗？通过代码来展示

在 Node.js 中创建一个 Http 服务是很简单的一件事情，我们可以通过 HTTP 模块来完成这些操作。

```js
const http = require('http');
const server = http.createServer((request, response) => {
    if (request.url === '/hello')  {
        response.writeHead(200, {
            'Content-Type': 'text/plain',
        });
        response.end('hello world!');
    } else {
        response.end('OK!');
    }
});

server.listen(3000, '127.0.0.1', () => {
    console.log('service is listening at http://127.0.0.1:3000');
});
```