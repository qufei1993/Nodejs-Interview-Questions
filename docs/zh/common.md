
# 基础

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

这里推荐您使用 nvm 工具安装，方便后期的升级进行 Node.js 版本管理，以下为安装步骤：

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

![](../img/node-hello-world.png)


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

## Q6：Node.js 的核心组件有哪些？

Node.js 的核心组建是系统 API、V8 引擎和 Libuv。

**Libuv 库**

libuv 库是一个跨平台的支持事件驱动的 I/O 库。它是使用 C 和 C++ 语言为 Node.js 所开发的。但是它也被应用于 Mozilla's 的 Rust、Luvit、Julia、pyuv 等其它的语言。

libuv 库是 I/O 操作的核心部分，例如读取文件和 OS 交互。

关于 Libuv 的学习，可以参考 [libuv中文教程](https://github.com/luohaha/Chinese-uvbook/)

**V8 引擎**

来自于谷歌：“V8 是谷歌开源的高性能 JavaScript 引擎”，使用 C++ 开发，并在谷歌浏览器中使用。V8 中实现的 ECMAScript 中指定 ECMA - 262 ，第 3版运行在 Windows XP 和 Vista、Mac OS X 的 10.5 和 Linux 系统使用 IA - 32 或 ARM/MIPS 处理器。V8 可以独立运行，也可以嵌入到任何 C++ 应用程序。

如果你感兴趣想学习更多的 V8 引擎，请访问 [What is V8?](https://v8.dev/)

**APIs (NodeJS Core Libs)**

Node.js APIs 是根据您这请求去调用一些函数执行一些业务操作。默认情况下 Node.js 的 APIs 都是异步的，但是你想同步使用也是可以的（**同步方式是不推荐的**）。

例如，这个 fs 模块可以使用同步方式也可以使用异步方式。

```js
var fs = require('fs');  
fs.readFile('/files/help.txt', function(err, buf) {  
    // use fs.readFileSync() for sync operation. console.log(buf.toString());  
});   
```

Source: [Introduction to NodeJS, A SSJS: Part I - Components Explained](https://www.c-sharpcorner.com/UploadFile/dbd951/introduction-to-nodejs-a-ssjs-part-i/)

## Q7：什么是“回调地狱”及如何避免它？

“回调地狱”是指严重的回调嵌套，这些回调嵌套使得代码变得难以阅读和维护。

以下是回调嵌套的示例：

```js
query("SELECT clientId FROM clients WHERE clientName='picanteverde';", function(id){
  query(`SELECT * FROM transactions WHERE clientId=${id}`, function(transactions){
    transactions.each((transac) => {
      query(`UPDATE transactions SET value = ${transac.value*0.1} WHERE id=${transac.id}`, (error) => {
        if(!error){
          console.log("success!!");
        }else{
          console.log("error");
        }
      });
    });
  });
});
```

在某种程度上，修复“回调地狱”的方式是模块化。回调被分解为独立的函数，这些函数可以通过参数进行传递。所以，针对以上代码的第一个改进如下所示：

```js
const logError = (error) => {
    if(!error){
      console.log("success!!");
    }else{
      console.log("error");
    }
},
  updateTransaction = (t) => {
    query(`UPDATE transactions SET value = ${t.value*0.1} WHERE id=${t.id}, logError);
},
  handleTransactions = (transactions) => {
    transactions.each(updateTransaction);
},
  handleClient = (id) => {
    query(`SELECT * FROM transactions WHERE clientId=${id}`, handleTransactions);
};

query("SELECT clientId FROM clients WHERE clientName='picanteverde';",handleClient);
```

尽管这个代码相比第一个示例更容易易读，而且我们创建的的函数还可以得到复用。但是在某些情况下，我们想要使程序更健壮可通过 Promise 来解决。Promises allow additional desirable behavior such as error propagation and chaining. Node.js includes native support for them.

此外，generators 也提供了强大了回调地狱解决方案，使用它可以解决不同回调之间的依赖关系。然而 generators 会更高级一些使用起来会复杂一些。关于 Generators 更多信息可以阅读这篇文章 [Generators in Node.js](http://strongloop.com/strongblog/how-to-generators-node-js-yield-use-cases/)

然而，以上的虽然能很好解决回调地狱问题，但是目前有了更好的方案 Async/Await。使用 Async/Await 需要注意 Node.js 版本要在 v7.5 版本之上。

Source: [8 Essential Node.js Interview Questions](https://www.toptal.com/nodejs/interview-questions)
