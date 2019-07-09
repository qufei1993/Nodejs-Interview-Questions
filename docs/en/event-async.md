# Event/Async

## Q1: What are the timing features of Node.js?

The Timers module in Node.js contains functions that execute code after a set period of time.

- **setTimeout/clearTimeout** - can be used to schedule code execution after a designated amount of milliseconds

- **setInterval/clearInterval** - can be used to execute a block of code multiple times

- **setImmediate/clearImmediate** - will execute code at the end of the current event loop cycle

- **process.nextTick** - used to schedule a callback function to be invoked in the next iteration of the Event Loop

**Order of execution for timer**

```
process.nextTick > setImmidate > setTimeout / SetInterval
```

**Example**

```js
function cb(msg){
    return function() {
        console.log(msg);
    }
}

setTimeout(cb('setTimeout'), 1000);
setImmediate(cb('setImmediate'))
process.nextTick(cb('process.nextTick'));
cb('Main process')();
```

Output:

```bash
Main process
process.nextTick
setImmediate
setTimeout
```

Source: [https://dev.to/aershov24/7-hardest-nodejs-interview-questions--answers-3lje](https://dev.to/aershov24/7-hardest-nodejs-interview-questions--answers-3lje)

## Q2: What is the difference between process.nextTick and setTimeout recursive calls?

process.nextTick belongs to the micro-task, which is triggered at the end of the current execution stack, before EventLoop. The following two are recursive calls. The process.nextTick in test1 is called in the current execution stack, which is one-time execution, equivalent to ```while(true ){}``` , the main thread is stuck in an infinite loop, blocking IO operations.

In the test2 method, setTimeout is a macro task, and it is also recursive in the task queue, but it is not a one-time execution but multiple Event Loops, which will not block IO operations. Also note that setTimeout has a minimum time of 4ms.

```js
function test1() {
    process.nextTick(() => test()); // Infinite loop
}

function test2() {
    setTimeout(() => test(), 0); // Timing execution does not fall into an infinite loop
}
```

process.nextTick will block IO, setImmediate will not output

```js
function test() {
    return process.nextTick(() => test());
}

test();

setImmediate(() => {
    console.log('setImmediate');
})
```

The following use setTimeout will not cause IO blocking, will output setImmediate

```js
function test() { 
    setTimeout(() => test(), 0);
}

test()

setImmediate(() => {
    console.log('setImmediate');
})

// setImmediate
```



## Q3: Explain event loop architecture of JavaScript.

As we know, JavaScript runs in a single thread. When the user requests an operation, the runtime provides a callback (to the operation that will be executed once the operation is done) and moves on to the next step to do some other operation.

![](../img/javascript_eventloop.jpg)

When an event occurs, the corresponding event message is queued into the EventLoop
The Event Loop is checked for the message existence, if an event message exists then the corresponding callback is fired.
Once the callback is done with the job, control is returned to the event loop that continues from Step 2.
Note: if the JavaScript runtime is engaged with the event message, then the other messages must wait until the current message is done with the job. Sometimes you might get the browser saying something like “Browser is not responding”. This is because of the long-running event message. So try to keep the event messages as short as possible.

For example:

var msg = document.getElementById("msg");  
msg.addEventListener("click", function () {  
this.style.color = "blue";  
});  
In this example above, we have attached the click event to the anonymous function as the event handler. When we clicked on that msg element, an event message is queued into the Event Queue.

If there are messages in the queue, then the click event message waits for the other messages to be done. All the other messages are processed, the over click event message is processed and the result is rendered into the DOM.

So we are somewhat clear about the JavaScript's Event Loop. Let us move onto the NodeJS EventLoop.

Source: [Introduction to NodeJS, A SSJS: Part II - EventLoop Explained](https://www.c-sharpcorner.com/article/node-js-interview-questions-and-answers/)

## Q4: Explain event loop architecture of NodeJS.
As stated by Unknown, “Everything runs in parallel; except your code”. Yes, your code runs synchronously but the operations given by the code run by Node is asynchronous.

![](../img/nodejs_eventlop.jpg)

The preceding figure shows the event loop architecture of NodeJS.

* When a request arrives, it is queued up into the EventLoop with a JavaScript Closure that includes the event (request, response) and corresponding callback.
* The event is then given to the one worker thread if the job seems to take a long time to complete that is from a C++ Thread Pool, handled by the libuv library.
* Once the job is done, the corresponding callback is fired to return back the response to the Main thread.
* The Event Loop returns the response to the client.

For example:

```js
var fs = require('fs');  
fs.readFile('avator.png', function(avator) {  
    console.log(‘image is loaded…’);  
});  
fs.writeFile('log.txt', 'Done', function() {  
    console.log(‘Done !..’);  
});    
```

The execution flow goes as in the following:

* Our code tells the node to do the two tasks read() and write() and then takes a rest.
* The read() and write() operations are enqueued into the Event Loop and distributed the job to the worker thread.
* Once the worker threads are done with the job, it will fire the callbacks to return the response to the Event Loop.
* Then the Event Loop returns the response to the client.

The callbacks of read() and write() will be executed in order to which one completes first. But note that only one of the callbacks would be executed at a time. So that the Node environment ensures that there won't be a deadlock or race condition. So it makes sure that the NodeJS provides Non-blocking IO.

We could consider the Post Office environment for the NodeJS.

When we post (request) something, the Post Master (Node) asks Post Men to deliver (get the job done) the post to the corresponding address.

Once the post men deliver the posts, they are reporting to the Post Master that it's completed, one by one.

Even the Post Master may assign some work to the post man at the time of reporting if he is free.

Source: [Introduction to NodeJS, A SSJS: Part II - EventLoop Explained](https://www.c-sharpcorner.com/article/node-js-interview-questions-and-answers/)

## Q5: What is Event Loop and Event Emitter ?

Node.js supports concurrency with the help of events and callbacks even if it is single threaded application. Node thread keeps an event loop and whenever any task gets completed, that thread fires the corresponding event.

EventEmitter fires an event whenever any tasks gets completed, any error occurred, any new listener is added or any listener is removed. It provides properties like on and emit, where on is used to bind the function and emit is used to fire an event.

Source: [top-20-interview-questions-on-nodejs](https://www.codingdefined.com/2017/04/top-20-interview-questions-on-nodejs.html)

## Q6: What is “callback hell” and how can it be avoided?

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

Even though this code is much easier to read, and we created some functions that we can even reuse later, in some cases it may be appropriate to use a more robust solution in the form of promises. Promises allow additional desirable behavior such as error propagation and chaining. Node.js includes native support for them.

Additionally, a more supercharged solution to callback hell was provided by generators, as these can resolve execution dependency between different callbacks. However, generators are much more advanced and it might be overkill to use them for this purpose. To read more about generators you can start with [this post]((http://strongloop.com/strongblog/how-to-generators-node-js-yield-use-cases/)).

However, these approaches are pretty dated at this point. The current solution is to use async/await—an approach that leverages Promises and finally makes it easy to flatten the so-called “pyramid of doom” shown above.

Source: [8 Essential Node.js Interview Questions](https://www.toptal.com/nodejs/interview-questions)