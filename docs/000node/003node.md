### HTTP服务器原理解析

上面的案例中，第一行请求（require）Node.js自带的 http 模块，并且把它赋值给 http 变量。 

接下来我们调用http模块提供的函数： createServer 。这个函数会返回一个对象，这个对象有一个叫做 listen 的方法，这个方法有一个数值参数，指定这个HTTP服务器监听的端口号。

咱们暂时先不管 http.createServer 的括号里的那个函数定义。

我们本来可以用这样的代码来启动服务器并侦听8888端口：

```js
var http = require("http");

var server = http.createServer();
server.listen(8888);
```

这段代码只会启动一个侦听8888端口的服务器，它不做任何别的事情，甚至连请求都不会应答。