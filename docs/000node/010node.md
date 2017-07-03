### 路由给真正的请求处理程序

现在我们的HTTP服务器和请求路由模块已经如我们的期望，可以相互交流了，就像一对亲密无间的兄弟。

当然这还远远不够，路由，顾名思义，是指我们要针对不同的URL有不同的处理方式。例如处理/start的“业务逻辑”就应该和处理/upload的不同。

在现在的实现下，路由过程会在路由模块中“结束”，并且路由模块并不是真正针对请求“采取行动”的模块，否则当我们的应用程序变得更为复杂时，将无法很好地扩展。

我们暂时把作为路由目标的函数称为请求处理程序。现在我们不要急着来开发路由模块，因为如果请求处理程序没有就绪的话，再怎么完善路由模块也没有多大意义。

应用程序需要新的部件，因此加入新的模块 -- 已经无需为此感到新奇了。我们来创建一个叫做requestHandlers的模块，并对于每一个请求处理程序，添加一个占位用函数，随后将这些函数作为模块的方法导出：

```js
/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */

function start() {
  console.log("Request handler 'start' was called.");
}

function upload() {
  console.log("Request handler 'upload' was called.");
}

exports.start = start;
exports.upload = upload;
```

现在我们将一系列请求处理程序通过一个对象来传递，并且需要使用松耦合的方式将这个对象注入到`route()`函数中。

我们先将这个对象引入到主文件index.js中：

```js
/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */

//从`server`模块中导入server对象

let server = require('./server');
let router = require("./router");
let requestHandlers = require("./requestHandlers");

//对象构造
var handle = {}
handle["/"] = requestHandlers.start;
handle["/start"] = requestHandlers.start;
handle["/upload"] = requestHandlers.upload;

//启动服务器
server.start(router.route, handle);
```

虽然`handle`并不仅仅是一个“东西”（一些请求处理程序的集合），我还是建议以一个动词作为其命名，这样做可以让我们在路由中使用更流畅的表达式，稍后会有说明。

正如所见，将不同的URL映射到相同的请求处理程序上是很容易的：只要在对象中添加一个键为`"/"`的属性，对应`requestHandlers.start`即可，这样我们就可以干净简洁地配置`/start`和`"/"`的请求都交由`start`这一处理程序处理。

在完成了对象的定义后，我们把它作为额外的参数传递给服务器，为此将`server.js`修改如下：

```js
/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */

//请求（require）Node.js自带的 http 模块，并且把它赋值给 http 变量。
let http = require("http");

let url = require("url");


//用一个函数将之前的内容包裹起来
let start = (route,handle) => {
        //箭头函数
    let onRequest = (request, response) => {
        
        let pathname = url.parse(request.url).pathname;
        console.log("Request for " + pathname + " received.");
        route(handle,pathname);

        response.writeHead(200, {"Content-Type": "text/plain;charset=utf-8"});
        response.write("添加小精灵微信(ershiyidianjian),加入全栈部落");
        response.end();
    }
    //把函数当作参数传递
    http.createServer(onRequest).listen(8888);

    console.log("Server has started.");
    console.log("请在浏览器中打开 http://127.0.0.1:8888...");
}

exports.start = start;
```

这样我们就在`start()`函数里添加了`handle`参数，并且把`handle`对象作为第一个参数传递给了`route()`回调函数。

然后我们相应地在`route.js`文件中修改`route()`函数：

有了这些，我们就把服务器、路由和请求处理程序在一起了。现在我们启动应用程序并在浏览器中访问`http://127.0.0.1:8888/start`，以下日志可以说明系统调用了正确的请求处理程序：

```js
bogon:路由给真正的请求处理程序 yuechunli$ node index.js
Server has started.
请在浏览器中打开 http://127.0.0.1:8888...
Request for /start received.
About to route a request for /start
Request handler 'start' was called.
```

并且在浏览器中打开`http://127.0.0.1:8888/`可以看到这个请求同样被start请求处理程序处理了：

```js
bogon:路由给真正的请求处理程序 yuechunli$ node index.js
Server has started.
请在浏览器中打开 http://127.0.0.1:8888...
Request for / received.
About to route a request for /
Request handler 'start' was called.
```