### 不好的实现方式

- 修改`requestHandler.js`文件内容如下：

```js
/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */

function start() {
  console.log("Request handler 'start' was called.");
  return "Hello Start";
}

function upload() {
  console.log("Request handler 'upload' was called.");
  return "Hello Upload";
}

exports.start = start;
exports.upload = upload;
```
好的。同样的，请求路由需要将请求处理程序返回给它的信息返回给服务器。因此，我们需要将router.js修改为如下形式：

```js

/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */

function route(handle, pathname) {
  console.log("About to route a request for " + pathname);
  if (typeof handle[pathname] === 'function') {
    return handle[pathname]();
  } else {
    console.log("No request handler found for " + pathname);
    return "404 Not found";
  }
}

exports.route = route;
```

正如上述代码所示，当请求无法路由的时候，我们也返回了一些相关的错误信息。

最后，我们需要对我们的server.js进行重构以使得它能够将请求处理程序通过请求路由返回的内容响应给浏览器，如下所示：

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
        var content = route(handle, pathname)
        response.write(content);
        response.end();
    }
    //把函数当作参数传递
    http.createServer(onRequest).listen(8888);

    console.log("Server has started.");
}

exports.start = start;
```

如果我们运行重构后的应用，一切都会工作的很好：

- 请求`http://localhost:8888/start`,浏览器会输出`Hello Start`。
- 请求`http://localhost:8888/upload`会输出`Hello Upload`,
- 而请求`http://localhost:8888/foo` 会输出`404 Not found`。

好，那么问题在哪里呢？简单的说就是： 当未来有请求处理程序需要进行非阻塞的操作的时候，我们的应用就“挂”了。

没理解？没关系，下面就来详细解释下。