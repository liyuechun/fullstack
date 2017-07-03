### 服务端模块化

- 何为`模块`？

```js
let http = require("http");
...
http.createServer(...);
```

在上面的代码中，Node.js中自带了一个叫做“http”的模块，我们在我们的代码中请求它并把返回值赋给一个本地变量。

这把我们的本地变量变成了一个拥有所有 http 模块所提供的公共方法的对象。

给这种本地变量起一个和模块名称一样的名字是一种惯例，但是你也可以按照自己的喜好来：

```js
var foo = require("http");
...
foo.createServer(...);
```

- 如何自定义模块

**将`server.js`文件的内容改成下面的内容。**

```js
/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */

//请求（require）Node.js自带的 http 模块，并且把它赋值给 http 变量。
let http = require("http");

//用一个函数将之前的内容包裹起来
let start = () => {
        //箭头函数
    let onRequest = (request, response) => {
        console.log("Request received.");
        response.writeHead(200, {"Content-Type": "text/plain;charset=utf-8"});
        response.write("添加小精灵微信(ershiyidianjian),加入全栈部落");
        response.end();
    }
    //把函数当作参数传递
    http.createServer(onRequest).listen(8888);

    console.log("Server has started.");
    console.log("请在浏览器中打开 http://127.0.0.1:8888...");
}

//导出`server`对象，对象中包含一个start函数
//对象格式为
/**
 * {
 *    start
 * }
 */

//这个对象导入到其他文件中即可使用，可以用任意的名字来接收这个对象

exports.start = start;
```

**在`server.js`当前的文件路径下新建一个`index.js`文件。内容如下：**

```js
/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */

//从`server`模块中导入server对象

let server = require('./server');

//启动服务器
server.start();
```

**如下图所示运行`index.js`文件。**

![](http://osazvg3ch.bkt.clouddn.com/Snip20170630_8.png)
![](http://osazvg3ch.bkt.clouddn.com/Snip20170630_10.png)
![](http://osazvg3ch.bkt.clouddn.com/Snip20170630_11.png)

一切运行正常，上面的案例中，`server.js`就是自定义的模块。

