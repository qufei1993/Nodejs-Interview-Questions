# Test

## Q1: What's a test pyramid? Give an example!

A test pyramid describes the ratio of how many unit tests, integration tests and end-to-end test you should write.

![](../img/test_pyramid_for_node_js_unit_testing-1465216863453.png)

An example for an HTTP API may look like this:

lots of low-level unit tests for models (dependencies are stubbed),
fewer integration tests, where you check how your models interact with each other (dependencies are not stubbed),
less end-to-end tests, where you call your actual endpoints (dependencies are not stubbed).

Source: [https://blog.risingstack.com/node-js-interview-questions/](https://blog.risingstack.com/node-js-interview-questions/)

## Q2: What's a stub? Name a use case.

Stubs are functions/programs that simulate the behaviours of components/modules. Stubs provide canned answers to function calls made during test cases. Also, you can assert on with what these stubs were called.

A use-case can be a file read, when you do not want to read an actual file:

```js
var fs = require('fs');

var readFileStub = sinon.stub(fs, 'readFile', function (path, cb) {
  return cb(null, 'filecontent');
});

expect(readFileStub).to.be.called;
readFileStub.restore();
```

Source: [https://blog.risingstack.com/node-js-interview-questions/](https://blog.risingstack.com/node-js-interview-questions/)
