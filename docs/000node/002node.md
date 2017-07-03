### 一个基础的HTTP服务器

用VSCode创建一个`server.js`的文件，将文件保存到桌面的`nodejs入门`文件夹里面。

在`server.js`文件里面写入以下内容：

```js
let http = require("http");

http.createServer(function(request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}).listen(8888);
```

上面的代码就是一个完整的Node.js服务器，如下图所示，点击VSCode左下脚按钮，打开VSCode终端，在终端中输入`node server.js`来进行验证。

![](http://osazvg3ch.bkt.clouddn.com/vscode%E6%93%8D%E4%BD%9C%E6%95%88%E6%9E%9C%E5%9B%BE.png)

![](http://osazvg3ch.bkt.clouddn.com/yunxingxiaoguotu.png)

 如上图所示，一个基础的HTTP服务器搞定。