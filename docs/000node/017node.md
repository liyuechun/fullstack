### 处理文件上传

最后，我们来实现我们最终的用例：允许用户上传图片，并将该图片在浏览器中显示出来。

我们通过它能学到这样两件事情： 

- 如何安装外部Node.js模块
- 以及如何将它们应用到我们的应用中

这里我们要用到的外部模块是`Felix Geisendörfer`开发的`node-formidable`模块。它对解析上传的文件数据做了很好的抽象。 其实说白了，处理文件上传“就是”处理POST数据 —— 但是，麻烦的是在具体的处理细节，所以，这里采用现成的方案更合适点。

使用该模块，首先需要安装该模块。Node.js有它自己的包管理器，叫NPM。它可以让安装Node.js的外部模块变得非常方便。

首先在当前项目路径下面通过`npm init`创建`package.json`文件:

![](http://osazvg3ch.bkt.clouddn.com/WX20170701-102100@2x.png)

PS:在终端输入`npm init`后，一路回车即可。新增的`package.json`文件的内容如下：

```js
{
  "author" : "liyuechun",
  "description" : "",
  "license" : "ISC",
  "main" : "index.js",
  "name" : "fileupload",
  "scripts" : {
    "test" : "echo \"Error: no test specified\" && exit 1"
  },
  "version" : "1.0.0"
}
```

接下来，在终端输入如下命令安装`formidable`外部模块。
如下所示：

```js
liyuechun:fileupload yuechunli$ ls
index.js                requestHandlers.js      server.js
package.json            router.js
liyuechun:fileupload yuechunli$ npm install formidable
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN fileupload@1.0.0 No description
npm WARN fileupload@1.0.0 No repository field.

+ formidable@1.1.1
added 1 package in 1.117s
liyuechun:fileupload yuechunli$
```

`package.json`文件变化如下：

```js
{
  "author": "liyuechun",
  "description": "",
  "license": "ISC",
  "main": "index.js",
  "name": "fileupload",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "version": "1.0.0",
  "dependencies": {
    "formidable": "^1.1.1"
  }
}
```

项目整体变化如下图所示：

![](http://osazvg3ch.bkt.clouddn.com/WX20170701-103037@2x.png)

现在我们就可以用`formidable`模块了——使用外部模块与内部模块类似，用`require`语句将其引入即可：

```js
let formidable = require("formidable");
```

这里该模块做的就是将通过HTTP POST请求提交的表单，在Node.js中可以被解析。我们要做的就是创建一个新的IncomingForm，它是对提交表单的抽象表示，之后，就可以用它解析request对象，获取表单中需要的数据字段。

node-formidable官方的例子展示了这两部分是如何融合在一起工作的：

```js
let formidable = require('formidable'),
    http = require('http'),
    util = require('util');

http.createServer(function(req, res) {
  if (req.url == '/upload' && req.method.toLowerCase() == 'post') {
    // parse a file upload
    let form = new formidable.IncomingForm();
    form.parse(req, function(err, fields, files) {
      res.writeHead(200, {'content-type': 'text/plain'});
      res.write('received upload:\n\n');
      res.end(util.inspect({fields: fields, files: files}));
    });
    return;
  }

  // show a file upload form
  res.writeHead(200, {'content-type': 'text/html'});
  res.end(
    '<form action="/upload" enctype="multipart/form-data" '+
    'method="post">'+
    '<input type="text" name="title"><br>'+
    '<input type="file" name="upload" multiple="multiple"><br>'+
    '<input type="submit" value="Upload">'+
    '</form>'
  );
}).listen(8888);
```

如果我们将上述代码，保存到一个文件中，并通过node来执行，就可以进行简单的表单提交了，包括文件上传。然后，可以看到通过调用form.parse传递给回调函数的files对象的内容，如下所示：

```js
received upload:

{ fields: { title: 'Hello World' },
  files:
   { upload:
      { size: 1558,
        path: './tmp/1c747974a27a6292743669e91f29350b',
        name: 'us-flag.png',
        type: 'image/png',
        lastModifiedDate: Tue, 21 Jun 2011 07:02:41 GMT,
        _writeStream: [Object],
        length: [Getter],
        filename: [Getter],
        mime: [Getter] } } }
```

为了实现我们的功能，我们需要将上述代码应用到我们的应用中，另外，我们还要考虑如何将上传文件的内容（保存在`./tmp`目录中）显示到浏览器中。

我们先来解决后面那个问题： 对于保存在本地硬盘中的文件，如何才能在浏览器中看到呢？

显然，我们需要将该文件读取到我们的服务器中，使用一个叫fs的模块。

我们来添加`/showURL`的请求处理程序，该处理程序直接硬编码将文件`./tmp/test.png`内容展示到浏览器中。当然了，首先需要将该图片保存到这个位置才行。

将`requestHandlers.js`修改为如下形式：

```js
/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */

var querystring = require("querystring"),
    fs = require("fs");

function start(response, postData) {
  console.log("Request handler 'start' was called.");

  var body = '<html>'+
    '<head>'+
    '<meta http-equiv="Content-Type" '+
    'content="text/html; charset=UTF-8" />'+
    '</head>'+
    '<body>'+
    '<form action="/upload" method="post">'+
    '<textarea name="text" rows="20" cols="60"></textarea>'+
    '<input type="submit" value="Submit text" />'+
    '</form>'+
    '</body>'+
    '</html>';

    response.writeHead(200, {"Content-Type": "text/html"});
    response.write(body);
    response.end();
}

function upload(response, postData) {
  console.log("Request handler 'upload' was called.");
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("You've sent the text: "+
  querystring.parse(postData).text);
  response.end();
}

function show(response, postData) {
  console.log("Request handler 'show' was called.");
  fs.readFile("./tmp/test.png", "binary", function(error, file) {
    if(error) {
      response.writeHead(500, {"Content-Type": "text/plain"});
      response.write(error + "\n");
      response.end();
    } else {
      response.writeHead(200, {"Content-Type": "image/png"});
      response.write(file, "binary");
      response.end();
    }
  });
}

exports.start = start;
exports.upload = upload;
exports.show = show;
```

我们还需要将这新的请求处理程序，添加到`index.js`中的路由映射表中：

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
handle["/show"] = requestHandlers.show;

//启动服务器
server.start(router.route, handle);
```

重启服务器之后，通过访问`http://localhost:8888/show`看看效果：

![](http://osazvg3ch.bkt.clouddn.com/WX20170701-104512@2x.png)

原因是当前项目路径下面没有`./tmp/test.png`图片，我们在当前项目路径下面添加`tmp`文件夹，在往里面拖拽一张图片，命名为`test.png`。

![](http://osazvg3ch.bkt.clouddn.com/WX20170701-105040@2x.png)

再重新启动服务器，访问`http://localhost:8888/show`查看效果如下：

![](http://osazvg3ch.bkt.clouddn.com/WX20170701-105516@2x.png)


咱继续，从`server.js`开始 —— 移除对`postData`的处理以及`request.setEncoding` （这部分`node-formidable`自身会处理），转而采用将`request`对象传递给请求路由的方式：

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
        route(handle, pathname, response, request);
    }
    //把函数当作参数传递
    http.createServer(onRequest).listen(8888);

    console.log("Server has started.");
}

exports.start = start;
```

接下来是 `router.js` —— 我们不再需要传递`postData`了，这次要传递`request`对象：

```js

/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */

function route(handle, pathname, response, request) {
  console.log("About to route a request for " + pathname);
  if (typeof handle[pathname] === 'function') {
    handle[pathname](response, request);
  } else {
    console.log("No request handler found for " + pathname);
    response.writeHead(404, {"Content-Type": "text/html"});
    response.write("404 Not found");
    response.end();
  }
}

exports.route = route;
```

现在，`request`对象就可以在我们的`upload`请求处理程序中使用了。`node-formidable`会处理将上传的文件保存到本地/tmp目录中，而我们需要做的是确保该文件保存成`./tmp/test.png`。 没错，我们保持简单，并假设只允许上传PNG图片。

这里采用`fs.renameSync(path1,path2)`来实现。要注意的是，正如其名，该方法是同步执行的， 也就是说，如果该重命名的操作很耗时的话会阻塞。 这块我们先不考虑。

接下来，我们把处理文件上传以及重命名的操作放到一起，如下`requestHandlers.js`所示：

```js
/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */

var querystring = require("querystring"),
    fs = require("fs"),
    formidable = require("formidable");

function start(response) {
  console.log("Request handler 'start' was called.");

  var body = '<html>'+
    '<head>'+
    '<meta http-equiv="Content-Type" content="text/html; '+
    'charset=UTF-8" />'+
    '</head>'+
    '<body>'+
    '<form action="/upload" enctype="multipart/form-data" '+
    'method="post">'+
    '<input type="file" name="upload" multiple="multiple">'+
    '<input type="submit" value="Upload file" />'+
    '</form>'+
    '</body>'+
    '</html>';

    response.writeHead(200, {"Content-Type": "text/html"});
    response.write(body);
    response.end();
}

function upload(response, request) {
  console.log("Request handler 'upload' was called.");

  var form = new formidable.IncomingForm();
  console.log("about to parse");
  form.parse(request, function(error, fields, files) {
    console.log("parsing done");
    fs.renameSync(files.upload.path, "./tmp/test.png");
    response.writeHead(200, {"Content-Type": "text/html"});
    response.write("received image:<br/>");
    response.write("<img src='/show' />");
    response.end();
  });
}

function show(response) {
  console.log("Request handler 'show' was called.");
  fs.readFile("./tmp/test.png", "binary", function(error, file) {
    if(error) {
      response.writeHead(500, {"Content-Type": "text/plain"});
      response.write(error + "\n");
      response.end();
    } else {
      response.writeHead(200, {"Content-Type": "image/png"});
      response.write(file, "binary");
      response.end();
    }
  });
}

exports.start = start;
exports.upload = upload;
exports.show = show;
```

重启服务器，浏览器访问`http://127.0.0.1:8888`，效果图如下：
![](http://osazvg3ch.bkt.clouddn.com/WX20170701-111333@2x.png)

选择一张图片上传，查看效果：

![](http://osazvg3ch.bkt.clouddn.com/WX20170701-122242@2x.png)