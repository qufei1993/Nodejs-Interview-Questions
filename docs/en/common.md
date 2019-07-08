

## Q1: What is Node.js?

Node.js is a server side JavaScript platform which is built on Google Chrome’s JavaScript V8 engine. It is an open source and cross platform application to develop server side and networking application. Anyone can develop the Node.js application by written code in JavaScript and it can be run on Microsoft Windows, Linux or OS X.

Node.js is not a new language and also it is not just framework built on JavaScript. It is built on Chrome's JavaScript Runtime, so the code is written, execute very similarly to browser. 

**Features of Node.js**

Following are some important feature of Node.js

- **Fast in Code execution**: It is very fast in code execution. 

- **Highly scalable**: Node.js uses single thread model for event looping. Events are responded to the server without blocking other operation. This makes the Node.js highly scalable. Traditional servers Create limited threads to handle request and Node.js Creates single thread that provide service to much larger number of requests.

- **Event Driven and Asynchronous**: All API of Node.js is asynchronous. It means that server is moving the next API call without wait for returning data of previous request.

- **No Buffering**: Node.js is never buffering any data.

Many of us are confused about Node.js. It is not a web server like Apache. Node.js is providing new way to execute our code. It is JavaScript runtime. Node.js is provides facility to create HTTP server and we can host our application on same.

Source: [Introduction To Node.js](https://www.c-sharpcorner.com/article/introduction-to-node-js/)

## Q2: How can I install Node.js?

We can download Node.js software from the website [https://nodejs.org/en/](https://nodejs.org/en/).

**nvm install**

It is recommended that you use the nvm tool to install, which is convenient for later upgrade management. Here are the installation steps:

* nvm installation：wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
* view all Node.js versions：nvm ls-remote
* view local Node.js versions：nvm ls
* Node.js installation：nvm install v6.9.5
* set the system default Node.js version：nvm alias default v6.9.5

**Verifying Node.js Installation**

After successful installation of Node.js, you can check it’s working.

Open Command Prompt and write the following command:

```node
$ node
```

Next, at the Node.js prompt will appear. After the prompt is visible, write the following command:

console.log(“NeErAj KuMaR”); 

Press Enter

![](./img/node-interview/install-node-check.jpg)



## Q3: How can you listen on port 80 with Node?

Trick question! You should not try to listen with Node on port 80 (in Unix-like systems) - to do so you would need superuser rights, but it is not a good idea to run your application with it.

Still, if you want to have your Node.js application listen on port 80, here is what you can do. Run the application on any port above 1024, then put a reverse proxy like nginx in front of it.

## Q4: What is an error-first callback?

Error-first callbacks are used to pass errors and data as well. You have to pass the error as the first parameter, and it has to be checked to see if something went wrong. Additional arguments are used to pass data.

```js
fs.readFile(filePath, function(err, data) {  
    if (err) {
        // handle the error
        return console.log(err)
    }
  
    // return the data object
    return data;
})
```

## Q5: Can you create Http Server in Node.js, explain with code?

Yes, creating an Http Server in Node.js is very simple, we can do it through the HTTP module.

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