### 以非阻塞操作进行请求响应

我刚刚提到了这样一个短语 —— “正确的方式”。而事实上通常“正确的方式”一般都不简单。

不过，用Node.js就有这样一种实现方案： 函数传递。下面就让我们来具体看看如何实现。

到目前为止，我们的应用已经可以通过应用各层之间传递值的方式（请求处理程序 -> 请求路由 -> 服务器）将请求处理程序返回的内容（请求处理程序最终要显示给用户的内容）传递给HTTP服务器。

现在我们采用如下这种新的实现方式：相对采用将内容传递给服务器的方式，我们这次采用将服务器“传递”给内容的方式。 从实践角度来说，就是将response对象（从服务器的回调函数onRequest()获取）通过请求路由传递给请求处理程序。 随后，处理程序就可以采用该对象上的函数来对请求作出响应。

原理就是如此，接下来让我们来一步步实现这种方案。


先从server.js开始：

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
        route(handle, pathname, response);
    }
    //把函数当作参数传递
    http.createServer(onRequest).listen(8888);

    console.log("Server has started.");
}

exports.start = start;
```

相对此前从route()函数获取返回值的做法，这次我们将response对象作为第三个参数传递给route()函数，并且，我们将onRequest()处理程序中所有有关response的函数调都移除，因为我们希望这部分工作让route()函数来完成。

下面就来看看我们的router.js:

同样的模式：相对此前从请求处理程序中获取返回值，这次取而代之的是直接传递response对象。

如果没有对应的请求处理器处理，我们就直接返回“404”错误。

最后，我们将requestHandler.js修改为如下形式：

```js
/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */


//我们引入了一个新的Node.js模块，child_process。之所以用它，是为了实现一个既简单又实用的非阻塞操作：exec()。
var exec = require("child_process").exec;

function start(response) {
  console.log("Request handler 'start' was called.");

  exec("ls -lah", function (error, stdout, stderr) {
    response.writeHead(200, {"Content-Type": "text/plain"});
    response.write(stdout);
    response.end();
  });
}

function upload(response) {
  console.log("Request handler 'upload' was called.");
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello Upload");
  response.end();
}

exports.start = start;
exports.upload = upload;
```

我们的处理程序函数需要接收response参数，为了对请求作出直接的响应。

start处理程序在exec()的匿名回调函数中做请求响应的操作，而upload处理程序仍然是简单的回复“Hello World”，只是这次是使用response对象而已。

这时再次我们启动应用（node index.js），一切都会工作的很好。

在浏览器中打开 `http:127.0.0.0:8888/start` 效果图如下所示：

![](http://osazvg3ch.bkt.clouddn.com/Snip20170630_21.png)

在浏览器中打开 `http:127.0.0.0:8888/upload` 效果图如下所示：
![](http://osazvg3ch.bkt.clouddn.com/Snip20170630_22.png)

如果想要证明`/start`处理程序中耗时的操作不会阻塞对`/upload`请求作出立即响应的话，可以将`requestHandlers.js`修改为如下形式：

```js
var exec = require("child_process").exec;

function start(response) {
  console.log("Request handler 'start' was called.");

  exec("find /",
    { timeout: 10000, maxBuffer: 20000*1024 },
    function (error, stdout, stderr) {
      response.writeHead(200, {"Content-Type": "text/plain"});
      response.write(stdout);
      response.end();
    });
}

function upload(response) {
  console.log("Request handler 'upload' was called.");
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello Upload");
  response.end();
}

exports.start = start;
exports.upload = upload;
```

这样一来，当请求`http://localhost:8888/start`的时候，会花10秒钟的时间才载入，而当请求`http://localhost:8888/upload`的时候，会立即响应，纵然这个时候`/start`响应还在处理中。
