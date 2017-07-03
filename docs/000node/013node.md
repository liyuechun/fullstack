### 阻塞与非阻塞

我们先不解释这里`阻塞`与`非阻塞`，我们来修改下start请求处理程序，我们让它等待10秒以后再返回`Hello Start`。因为，JavaScript中没有类似sleep()这样的操作，所以这里只能够来点小Hack来模拟实现。

让我们将requestHandlers.js修改成如下形式：

```js
/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */

function start() {
  console.log("Request handler 'start' was called.");

  function sleep(milliSeconds) {
    var startTime = new Date().getTime();
    while (new Date().getTime() < startTime + milliSeconds);
  }

  sleep(10000);
  return "Hello Start";
}


function upload() {
  console.log("Request handler 'upload' was called.");
  return "Hello Upload";
}

exports.start = start;
exports.upload = upload;
```

上述代码中，我先调用了`upload()`，会和此前一样立即返回。当函数`start()`被调用的时候，Node.js会先等待10秒，之后才会返回“Hello Start”。如下图所示，等待中：

![](http://osazvg3ch.bkt.clouddn.com/Snip20170630_19.png)


（当然了，这里只是模拟休眠10秒，实际场景中，这样的阻塞操作有很多，比方说一些长时间的计算操作等。）

接下来就让我们来看看，我们的改动带来了哪些变化。

如往常一样，我们先要重启下服务器。为了看到效果，我们要进行一些相对复杂的操作（跟着我一起做）： 首先，打开两个浏览器窗口或者标签页。在第一个浏览器窗口的地址栏中输入http://localhost:8888/start， 但是先不要打开它！

在第二个浏览器窗口的地址栏中输入http://localhost:8888/upload， 同样的，先不要打开它！

接下来，做如下操作：在第一个窗口中（“/start”）按下回车，然后快速切换到第二个窗口中（“/upload”）按下回车。

注意，发生了什么： /start URL加载花了10秒，这和我们预期的一样。但是，/upload URL居然也花了10秒，而它在对应的请求处理程序中并没有类似于sleep()这样的操作！

这到底是为什么呢？原因就是start()包含了阻塞操作。形象的说就是“它阻塞了所有其他的处理工作”。

这显然是个问题，因为Node一向是这样来标榜自己的：“在node中除了代码，所有一切都是并行执行的”。

这句话的意思是说，Node.js可以在不新增额外线程的情况下，依然可以对任务进行并行处理 —— Node.js是单线程的。它通过事件轮询（event loop）来实现并行操作，对此，我们应该要充分利用这一点 —— 尽可能的避免阻塞操作，取而代之，多使用非阻塞操作。

然而，要用非阻塞操作，我们需要使用回调，通过将函数作为参数传递给其他需要花时间做处理的函数（比方说，休眠10秒，或者查询数据库，又或者是进行大量的计算）。

对于Node.js来说，它是这样处理的：“嘿，probablyExpensiveFunction()（译者注：这里指的就是需要花时间处理的函数），你继续处理你的事情，我（Node.js线程）先不等你了，我继续去处理你后面的代码，请你提供一个callbackFunction()，等你处理完之后我会去调用该回调函数的，谢谢！”

（如果想要了解更多关于事件轮询细节，可以阅读Mixu的博文——[理解node.js的事件轮询](http://blog.mixu.net/2011/02/01/understanding-the-node-js-event-loop/)。）

接下来，我们会介绍一种错误的使用非阻塞操作的方式。

和上次一样，我们通过修改我们的应用来暴露问题。

这次我们还是拿start请求处理程序来“开刀”。将其修改成如下形式：

```js
/**
 * 从零到壹全栈部落，添加小精灵微信(ershiyidianjian)
 */


//我们引入了一个新的Node.js模块，child_process。之所以用它，是为了实现一个既简单又实用的非阻塞操作：exec()。
var exec = require("child_process").exec;

function start() {
  console.log("Request handler 'start' was called.");

  /**
   * exec()做了什么呢？
   * 它从Node.js来执行一个shell命令。
   * 在本例子中，我们用它来获取当前目录下所有的文件（“ls -lah”）
   * 然后，当`/start` URL请求的时候将文件信息输出到浏览器中。
   * 下面的代码非常直观的： 
   * 创建了一个新的变量content（初始值为“empty”）。
   * 执行“ls -lah”命令，将结果赋值给content，最后将content返回。
   */
  var content = "empty";

  exec("ls -lah", function (error, stdout, stderr) {
    content = stdout;
  });

  return content;
}

function upload() {
  console.log("Request handler 'upload' was called.");
  return "Hello Upload";
}

exports.start = start;
exports.upload = upload;
```

和往常一样，我们启动服务器，然后访问“http://localhost:8888/start” 。

![](http://osazvg3ch.bkt.clouddn.com/Snip20170630_20.png)

载入一个漂亮的web页面，其内容为“empty”。怎么回事？

如果想要证明这一点，可以将“ls -lah”换成比如“find /”这样更耗时的操作来效果。

然而，针对浏览器显示的结果来看，我们并不满意我们的非阻塞操作，对吧？

好，接下来，我们来修正这个问题。在这过程中，让我们先来看看为什么当前的这种方式不起作用。

问题就在于，为了进行非阻塞工作，exec()使用了回调函数。

在我们的例子中，该回调函数就是作为第二个参数传递给exec()的匿名函数：

```js
function (error, stdout, stderr) {
  content = stdout;
}
```

现在就到了问题根源所在了：我们的代码是同步执行的，这就意味着在调用exec()之后，Node.js会立即执行 return content ；在这个时候，content仍然是“empty”，因为传递给exec()的回调函数还未执行到——因为exec()的操作是异步的。

我们这里“ls -lah”的操作其实是非常快的（除非当前目录下有上百万个文件）。这也是为什么回调函数也会很快的执行到 —— 不过，不管怎么说它还是异步的。

为了让效果更加明显，我们想象一个更耗时的命令： “find /”，它在我机器上需要执行1分钟左右的时间，然而，尽管在请求处理程序中，我把“ls -lah”换成“find /”，当打开/start URL的时候，依然能够立即获得HTTP响应 —— 很明显，当exec()在后台执行的时候，Node.js自身会继续执行后面的代码。并且我们这里假设传递给exec()的回调函数，只会在“find /”命令执行完成之后才会被调用。

那究竟我们要如何才能实现将当前目录下的文件列表显示给用户呢？

好，了解了这种不好的实现方式之后，我们接下来来介绍如何以正确的方式让请求处理程序对浏览器请求作出响应。
