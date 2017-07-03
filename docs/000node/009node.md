### 如何来进行请求的“路由”

我们要为路由提供请求的URL和其他需要的GET及POST参数，随后路由需要根据这些数据来执行相应的代码（这里“代码”对应整个应用的第三部分：一系列在接收到请求时真正工作的处理程序）。

因此，我们需要查看HTTP请求，从中提取出请求的URL以及GET/POST参数。这一功能应当属于路由还是服务器（甚至作为一个模块自身的功能）确实值得探讨，但这里暂定其为我们的HTTP服务器的功能。

我们需要的所有数据都会包含在request对象中，该对象作为onRequest()回调函数的第一个参数传递。但是为了解析这些数据，我们需要额外的Node.JS模块，它们分别是url和querystring模块。

```js
                               url.parse(string).query
                                           |
           url.parse(string).pathname      |
                       |                   |
                       |                   |
                     ------ -------------------
http://localhost:8888/start?foo=bar&hello=world
                                ---       -----
                                 |          |
                                 |          |
              querystring.parse(string)["foo"]    |
                                            |
                         querystring.parse(string)["hello"]
                         
```

当然我们也可以用querystring模块来解析POST请求体中的参数，稍后会有演示。

现在我们来给onRequest()函数加上一些逻辑，用来找出浏览器请求的URL路径：

![](http://osazvg3ch.bkt.clouddn.com/Snip20170630_12.png)

接下来我在终端执行`node index.js`命令，如下所示：

```js
bogon:如何来进行请求的“路由” yuechunli$ node index.js
Server has started.
请在浏览器中打开 http://127.0.0.1:8888...
```

我先在`Safari`浏览器中打开`http://127.0.0.1:8888`，浏览器展示效果如下：
![](http://osazvg3ch.bkt.clouddn.com/Snip20170630_13.png)

控制台效果如下：

```js
bogon:如何来进行请求的“路由” yuechunli$ node index.js
Server has started.
请在浏览器中打开 http://127.0.0.1:8888...
Request for / received.
```

接着我在`Google`浏览器里面打开 http://127.0.0.1:8888... ，浏览器效果图如下：

![](http://osazvg3ch.bkt.clouddn.com/Snip20170630_17.png)

控制台效果如下：

![](http://osazvg3ch.bkt.clouddn.com/Snip20170630_18.png)

为什么在`Safari`浏览器中进行请求时，只打印了一个`Request for / received.`,而在`Google`浏览器中访问时，会多打印一个`Request for /favicon.ico received.`，如上图所示，原因是因为在`Google`浏览器中，浏览器的原因会去尝试请求`favicon.ico`小图标。

为了演示效果，还有不受`Google`浏览器的`favicon.ico`请求的干扰，我接着在`Safari`里面请求`http://127.0.0.1:8888/start`和`http://127.0.0.1:8888/upload`，我们看看控制台展示的内容是什么。

```js
bogon:如何来进行请求的“路由” yuechunli$ node index.js
Server has started.
请在浏览器中打开 http://127.0.0.1:8888...
Request for /start received.
Request for /upload received.
```

好了，我们的应用现在可以通过请求的URL路径来区别不同请求了--这使我们得以使用路由（还未完成）来将请求以URL路径为基准映射到处理程序上。

在我们所要构建的应用中，这意味着来自`/start`和`/upload`的请求可以使用不同的代码来处理。稍后我们将看到这些内容是如何整合到一起的。

现在我们可以来编写路由了，建立一个名为`router.js`的文件，添加以下内容：

```js
/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */

function route(pathname) {
  console.log("About to route a request for " + pathname);
}

exports.route = route;
```

如你所见，这段代码什么也没干，不过对于现在来说这是应该的。在添加更多的逻辑以前，我们先来看看如何把路由和服务器整合起来。

首先，我们来扩展一下服务器的`start()`函数，以便将路由函数作为参数传递过去：

```js
/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */

//请求（require）Node.js自带的 http 模块，并且把它赋值给 http 变量。
let http = require("http");

let url = require("url");


//用一个函数将之前的内容包裹起来
let start = (route) => {
        //箭头函数
    let onRequest = (request, response) => {
        
        let pathname = url.parse(request.url).pathname;
        console.log("Request for " + pathname + " received.");
        route(pathname);
        
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

同时，我们会相应扩展`index.js`，使得路由函数可以被注入到服务器中：

```js
/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */

//从`server`模块中导入server对象

let server = require('./server');
let router = require("./router");

//启动服务器
server.start(router.route);
```

在这里，我们传递的函数依旧什么也没做。

如果现在启动应用（node index.js，始终记得这个命令行），随后请求一个URL，你将会看到应用输出相应的信息，这表明我们的HTTP服务器已经在使用路由模块了，并会将请求的路径传递给路由：

```js
bogon:如何来进行请求的“路由” v2.0 yuechunli$ node index.js
Server has started.
请在浏览器中打开 http://127.0.0.1:8888...
Request for / received.
About to route a request for /
```
