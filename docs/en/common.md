
# Common

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

![](../img/node-hello-world.png)

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

## Q6: What are the main components of Node.js?

The main components of NodeJs are APIs, a V8 Engine and Libuv.

**Libuv Library**

Libuv is a multi-platform support library for asynchronous I/O. It was developed for Node.js using C and C++. But it's also used by Mozilla's Rust language, Luvit, Julia, pyuv and others.

This libuv library is the main part for I/O related operations like reading files and interacting with the OS.

You can check it out on GitHub for more information about [the libuv library](https://github.com/nikhilm/uvbook).

**V8 Engine**

From Google: “V8 is Google's open-source high-performance JavaScript engine, written in C++ and used in Google Chrome, the open source browser from Google. It implements ECMAScript as specified in ECMA-262, 3rd edition and runs on Windows XP and Vista, Mac OS X 10.5+, and Linux systems that use IA-32, ARM or MIPS processors. V8 can run standalone, or can be embedded into any C++ application”.

If you are interested in learning more about the V8 engine, please visit [here](https://v8.dev/).

**APIs (NodeJS Core Libs)**

The NodeJs APIs are nothing but functions to do something upon your request. By default the NodeJS apis are asynchronous in nature but still you can use NodeJS APIs synchronously.

For example, the fs module could be used either synchronously or asynchronously.

```js
var fs = require('fs');  
fs.readFile('/files/help.txt', function(err, buf) {  
    // use fs.readFileSync() for sync operation. console.log(buf.toString());  
});   
```

Source: [Introduction to NodeJS, A SSJS: Part I - Components Explained](https://www.c-sharpcorner.com/UploadFile/dbd951/introduction-to-nodejs-a-ssjs-part-i/)

## Q7: What is “callback hell” and how can it be avoided?

“Callback hell” refers to heavily nested callbacks that have become unweildy or unreadable.

An example of heavily nested code is below:

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

At one point, the primary method to fix callback hell was modularization. The callbacks are broken out into independent functions which can be called with some parameters. So the first level of improvement might be:

```js
const logError = (error) => {
    if(!error){
      console.log("success!!");
    }else{
      console.log("error");
    }
},
  updateTransaction = (t) => {
    query(`UPDATE transactions SET value = ${t.value*0.1} WHERE id=${t.id}`, logError);
},
  handleTransactions = (transactions) => {
    transactions.each(updateTransaction);
},
  handleClient = (id) => {
    query(`SELECT * FROM transactions WHERE clientId=${id}`, handleTransactions);
};

query("SELECT clientId FROM clients WHERE clientName='picanteverde';",handleClient);
```

Even though this code is much easier to read, and we created some functions that we can even reuse later, in some cases it may be appropriate to use a more robust solution in the form of promises. Promises allow additional desirable behavior such as error propagation and chaining. Node.js includes native support for them.

Additionally, a more supercharged solution to callback hell was provided by generators, as these can resolve execution dependency between different callbacks. However, generators are much more advanced and it might be overkill to use them for this purpose. To read more about generators you can start with [this post]((http://strongloop.com/strongblog/how-to-generators-node-js-yield-use-cases/)).

However, these approaches are pretty dated at this point. The current solution is to use async/await—an approach that leverages Promises and finally makes it easy to flatten the so-called “pyramid of doom” shown above.

Source: [8 Essential Node.js Interview Questions](https://www.toptal.com/nodejs/interview-questions)

## Q8: What are the event-Driven Programming of Node.js?

Event-Driven Programming is the term where the flow of the code is determined by events (click, load and so on). It's one of the basic milestones of today's popular programming languages, such as C#, Java and many more; I won't dwell on all of them here. In Node.js and moreover in any kind of JavaScript project, you'll be using or have used event-driven processes. Whether it's a page onload or a button click event, this is something you have done, whether you knew it or not.

Let's make an example to the classic event-driven process and how its done in NodeJS:

result = getJSONfromDestination();  
binddata(result);  
The operation above requires a blocking I/O process (single-threaded operation that waits for the previously working synchronous code).

Now let's have a look at how we do the asynchronous way (a non-blocking I/O process).

json_finished = function(result){  
binddata(result);  
}  
  
getJSONfromDestination(jsonfinished);  
As you can see, this is a non-blocking sample, because json_finished does not work first as you can imagine.
It starts working when you call the getJSONfromDestination method and send param as the function to json_finished.

Source: [NodeJS Series #6: Event - Driven Programming](https://www.c-sharpcorner.com/UploadFile/iersoy/nodejs-series-sharp6-event-driven-programming/)

## Q9: What is NPM? What is the need of NPM in Node.js?

NPM stands for Node.js Package Management. It comes with Node.js platform and allows us to install various packages for Node.js. This package manages and supports various commands to install and remove the modules. Here one important note is we require either package.json file or the node_modules folder to install modules locally.

One of the best things about npm is that it locally stores all dependencies. For example, if module X uses module A version 1.0, and module Y uses module A version 1.5, then both X and Y will have their own local copies of module A.
 
```js
// Module X
{
  "name": "X",
  "dependencies": {
    "A": "^1.0"
  }
}
```

```js
// Module Y
{
  "name": "Y",
  "dependencies": {
    "A": "^1.5"
  }
}
```

**Need of npm package**
 
When we are developing some Node.js projects, we may encounter some places that need NPM, such as linking Redis, MongoDB, or sending a request Request. These modules can make us more focused on business development, of course, sometimes you will have something special. The need, at this time, may need to encapsulate an NPM module to achieve reuse.

Source: [How to Create Nodejs Module and Publish Over to Npm](https://www.c-sharpcorner.com/UploadFile/g_arora/how-to-create-nodejs-module-and-publish-over-to-npm/)

## Q10: What does Node.js do? 10 application scenarios for Node.js?

Web Server Backend (Java, PHP do node. JS can Do)、Command-line tools，Now count the 10 scenarios of Node.js:

1. Web Development: Express + EJS + MongoDB(mongoose)/Mysql
2. REST Development: Restify
3. Web chat Room (IM): Express + Socket.io
4. Web Crawler: Cheerio/request
5. Web Blog: Hexo
6. Web Forum: Nodeclub
7. Web Slideshow: Cleaver
8. Front-end package management platform: bower.js
9. OAuth Certification: Passport
10. Timer Task Tool: Later

Source: [What does node. js do? 10 application scenarios for node. js](https://topic.alibabacloud.com/a/what-does-node-js-do-10-application-scenarios-for-node-js_4_87_30002440.html)


## Q11: What is LTS releases of Node.js why should you care?

An LTS(Long Term Support) version of Node.js receives all the critical bug fixes, security updates and performance improvements.

LTS versions of Node.js are supported for at least 18 months and are indicated by even version numbers (e.g. 4, 6, 8). They’re best for production since the LTS release line is focussed on stability and security, whereas the Current release line has a shorter lifespan and more frequent updates to the code. Changes to LTS versions are limited to bug fixes for stability, security updates, possible npm updates, documentation updates and certain performance improvements that can be demonstrated to not break existing applications.

Source: [github.com/i0natan/nodebestpractices](github.com/i0natan/nodebestpractices)