# 模块

## Q1：require 的加载机制？

在 Nodejs 中模块加载一般会经历3个步骤，路径分析、文件定位、编译执行。初次加载之后会进行缓存，下次直接从系统缓存加载。更多内容参考 [Node.js 模块加载机制](https://www.nodejs.red/#/nodejs/module?id=%e6%a8%a1%e5%9d%97%e5%8a%a0%e8%bd%bd%e6%9c%ba%e5%88%b6)

## Q2：module.exports 与 exports 的区别?

exports 相当于 module.exports 的快捷方式如下所示:

```js
const exports = modules.exports;
```

但是要注意不能改变 exports 的指向，我们可以通过 exports.test = 'a' 这样来导出一个对象, 但是不能向下面示例直接赋值，这样会改变 exports 的指向，相当于修改了其引用关系。

```js
// 错误的写法 将会得到 undefined
exports = {
  'a': 1,
  'b': 2
}

// 正确的写法
modules.exports = {
  'a': 1,
  'b': 2
}
```

## Q3：假设有 a.js、b.js 两个模块相互引用，会有什么问题？是否为陷入死循环？

两个模块相互引用，假设 a、b 两个模块，在 a 模块中应用了 b 模块，之后 b 模块又引用了 a 模块，那么后一个模块引用的是前一个模块未完成的副本，并不会导致死循环，看一下示例：

```js
// a.js
console.log('a模块start');

exports.test = 1;

undeclaredVariable = 'a模块未声明变量'

const b = require('./b');

console.log('a模块加载完毕: b.test值：',b.test);
```

```js
// b.js
console.log('b模块start');

exports.test = 2;

const a = require('./a');

console.log('undeclaredVariable: ', undeclaredVariable);

console.log('b模块加载完毕: a.test值：', a.test);
```

控制台执行 ```node a.js``` 查看输出结果：

```bash
a模块start
b模块start
undeclaredVariable:  a模块未声明变量
b模块加载完毕: a.test值： 1
a模块加载完毕: b.test值： 2
```

启动 ```a.js``` 的时候，会加载 ```b.js```，那么在 ```b.js``` 中又加载了 ```a.js```，但是此时 ```a.js``` 模块还没有执行完，返回的是一个 ```a.js``` 模块的 ```exports``` 对象 ```未完成的副本``` 给到 ```b.js``` 模块。然后 ```b.js``` 完成加载之后将 ```exports``` 对象提供给了 ```a.js``` 模块