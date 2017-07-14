## 编写HelloWorld程序

- 查阅`app.js`源码

```js
var express = require('express');
var path = require('path');
var favicon = require('serve-favicon');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');

var index = require('./routes/index');
var users = require('./routes/users');

var app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');

// uncomment after placing your favicon in /public
app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', index);
app.use('/users', users);

// catch 404 and forward to error handler
app.use(function(req, res, next) {
  var err = new Error('Not Found');
  err.status = 404;
  next(err);
});

// error handler
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

console.log("Express server listening on port 3000...");

module.exports = app;
```

- `app.js`中添加代码

现在，我们写点有用的。我们不会直接在我们的index页面里写“Hello World!”，我们借这个机会学习一下如何使用route路由，同时学习一下Ejs引擎是如何工作的。在上面的app.js文件中添加**`如下`**两行代码：

![](http://oswrtif8l.bkt.clouddn.com/WechatIMG144.jpeg)

- 创建`helloworld.js`文件

![](http://oswrtif8l.bkt.clouddn.com/WechatIMG145.jpeg)

内容如下：

```js
var express = require('express');
var router = express.Router();

/* GET HelloWorld page. */
router.get('/', function(req, res, next) {
  res.render('helloworld', { title: 'HelloWorld' });
});

module.exports = router;
```

- 创建`helloworld.ejs`文件

![](http://oswrtif8l.bkt.clouddn.com/WechatIMG146.jpeg)

内容如下：

```js
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>
  </body>
</html>
```


- 运行程序
`npm start`启动服务器,然后在浏览器打开`http://localhost:3000/helloworld`,应该能看到这个漂亮的界面了：
![](http://oswrtif8l.bkt.clouddn.com/WechatIMG147.jpeg)

- ejs⚠️去除

![](http://oswrtif8l.bkt.clouddn.com/WechatIMG148.jpeg)

如上图所示，在VSCode中使用`ejs`代码会出现语法不识别的问题，启动VSCode，通过快捷键`⌘+P`快速打开窗口，将如下代码拷贝粘贴到里面，回车安装插件，然后重启即可。

```js
ext install ejs-language-support
```

[EJS Language Support](https://github.com/gregory-m/ejs-tmbundle)