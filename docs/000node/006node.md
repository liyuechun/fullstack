### 基于事件驱动的回调

事件驱动是Node.js原生的工作方式，这也是它为什么这么快的原因。

当我们使用`http.createServer`方法的时候，我们当然不只是想要一个侦听某个端口的服务器，我们还想要它在服务器收到一个HTTP请求的时候做点什么。

我们创建了服务器，并且向创建它的方法传递了一个函数。无论何时我们的服务器收到一个请求，这个函数就会被调用。

这个就是传说中的回调 。我们给某个方法传递了一个函数，这个方法在有相应事件发生时调用这个函数来进行回调 。

我们试试下面的代码：

```js
/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */

//请求（require）Node.js自带的 http 模块，并且把它赋值给 http 变量。
let http = require("http");

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
```

![](http://osazvg3ch.bkt.clouddn.com/0001.png)

在上图中，当我们执行`node server.js`命令时，Server has started.正常往下执行。

我们看看当我们在浏览器里面打开`http://127.0.0.1:8888`时会发生什么。

![](http://osazvg3ch.bkt.clouddn.com/0002.png)
![](http://osazvg3ch.bkt.clouddn.com/0003.png)

大家会发现在浏览器中打开`http://127.0.0.1:8888`时，在终端会输出`Request received.`,浏览器会输出`添加小精灵微信(ershiyidianjian),加入全栈部落`这一句话。

请注意，当我们在服务器访问网页时，我们的服务器可能会输出两次“Request received.”。那是因为大部分浏览器都会在你访问 http://localhost:8888/ 时尝试读取 http://localhost:8888/favicon.ico )

![](http://osazvg3ch.bkt.clouddn.com/0004.png)