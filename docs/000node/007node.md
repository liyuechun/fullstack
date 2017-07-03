### 服务器是如何处理请求的

好的，接下来我们简单分析一下我们服务器代码中剩下的部分，也就是我们的回调函数`onRequest()`的主体部分。

当回调启动，我们的`onRequest()`函数被触发的时候，有两个参数被传入：`request` 和`response` 。

它们是对象，你可以使用它们的方法来处理HTTP请求的细节，并且响应请求（比如向发出请求的浏览器发回一些东西）。

所以我们的代码就是：当收到请求时，使用`response.writeHead()`函数发送一个HTTP状态200和HTTP头的内容类型（content-type），使用`response.write()`函数在HTTP相应主体中发送文本`添加小精灵微信(ershiyidianjian),加入全栈部落`。

最后，我们调用 `response.end()` 完成响应。

目前来说，我们对请求的细节并不在意，所以我们没有使用 `request` 对象。