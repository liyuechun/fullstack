往数据库里写入数据也不是很困难。我们需要一个route来接收POST请求而不是GET。我们可以使用Ajax，这是我最常用的方式。不过那可能需要另外一篇教程了，所以这里我们还是用最原始的post手段。当然，要记住，如果你愿意，用ajax并不难。

## 第1步 – 建立你的数据输入页面

这里我们需要快一点：建立两个丑陋的不加样式的文本框和一个提交按钮。像上面一样，我们从`app.use()`开始。打开`app.js`找到下面的代码：

```js
var index = require('./routes/index');
var users = require('./routes/users');
var helloworld = require('./routes/helloworld');
var userlist = require('./routes/userlist');
```

在下面加上：

```js
var newuser = require('./routes/newuser');
```

再找到下面的代码：

```js
app.use('/', index);
app.use('/users', users);
app.use('/helloworld', helloworld);
app.use('/userlist', userlist);
```

在下面加上：

```js
app.use('/newuser', newuser);
```

在`routs`文件夹中创建`newuser.js`文件，内容如下：

```js
var express = require('express');
var router = express.Router();

/* GET users listing. */
router.get('/', function(req, res, next) {
  res.render('newuser', { title: 'Add New User' });
});

module.exports = router;
```

在`views`文件夹中创建`newuser.ejs`文件，内容如下：

```html
<!DOCTYPE html>
<html>

<head>
    <title>Add User</title>
    <link rel="stylesheet" href="/stylesheets/style.css" />
</head>

<body>
    <h1>
        <%= title %>
    </h1>
    <form name="adduser" method="post" action="/adduser">
        <input type="text" placeholder="username" name="username" />
        <input type="text" placeholder="useremail" name="useremail" />
        <input type="submit" value="submit" />
    </form>
</body>

</html>
```

这里我们创建一个form，method是post，action是adducer。简单明了。下面我们定义了两个输入框和一个提交按钮。启动服务器，浏览器打开`http://localhost:3000/newuser`效果图如下：

![](http://om1c35wrq.bkt.clouddn.com/WechatIMG152.jpeg)

点提交按钮，会出现如下错误，我们来修正错误。

```text
Not Found

404

Error: Not Found
    at /Users/liyuechun/Desktop/60MINUTES/nodetest1/app.js:36:13
    at Layer.handle [as handle_request] (/Users/liyuechun/Desktop/60MINUTES/nodetest1/node_modules/express/lib/router/layer.js:95:5)
    at trim_prefix (/Users/liyuechun/Desktop/60MINUTES/nodetest1/node_modules/express/lib/router/index.js:317:13)
    at /Users/liyuechun/Desktop/60MINUTES/nodetest1/node_modules/express/lib/router/index.js:284:7
    at Function.process_params (/Users/liyuechun/Desktop/60MINUTES/nodetest1/node_modules/express/lib/router/index.js:335:12)
    at next (/Users/liyuechun/Desktop/60MINUTES/nodetest1/node_modules/express/lib/router/index.js:275:10)
    at /Users/liyuechun/Desktop/60MINUTES/nodetest1/node_modules/express/lib/router/index.js:635:15
    at next (/Users/liyuechun/Desktop/60MINUTES/nodetest1/node_modules/express/lib/router/index.js:260:14)
    at Function.handle (/Users/liyuechun/Desktop/60MINUTES/nodetest1/node_modules/express/lib/router/index.js:174:3)
    at router (/Users/liyuechun/Desktop/60MINUTES/nodetest1/node_modules/express/lib/router/index.js:47:12)
```