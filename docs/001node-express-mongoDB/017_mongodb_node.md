## 把mongo连接到node

现在我们来建立一个页面，把数据库里的记录显示成一个漂亮的表格。这是我们准备生成的HTML内容：

```js
<ul>
    <li><a href="mailto:liyuechun@cldy.org">liyuechun</a></li>
    <li><a href="mailto:yanan@testdomain.com">yanan</a></li>
    <li><a href="mailto:fujinliang@testdomain.com">fujinliang</a></li>
    <li><a href="mailto:shenpingping@testdomain.com">shenpingping</a></li>
</ul>
```

打开`nodetest1`项目的`app.js`，现在接着往下看：

```js
var index = require('./routes/index');
var users = require('./routes/users');
var helloworld = require('./routes/helloworld');
```

下面添加一行：

```js
var userlist = require('./routes/userlist');
```

继续，找到下面代码的位置：

```js
app.use('/', index);
app.use('/users', users);
app.use('/helloworld', helloworld);
```

下面添加一行：

```js
app.use('/userlist', userlist);
```
