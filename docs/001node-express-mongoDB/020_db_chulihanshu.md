## 创建你的数据库处理函数

打开`app.js`文件，找到下面的代码：

```js
var index = require('./routes/index');
var users = require('./routes/users');
var helloworld = require('./routes/helloworld');
var userlist = require('./routes/userlist');
var newuser = require('./routes/newuser');
```

在下面添加一行：

```js
var adduser = require('./routes/adduser');
```

再次找到下面的代码：

```js
app.use('/', index);
app.use('/users', users);
app.use('/helloworld', helloworld);
app.use('/userlist', userlist);
app.use('/newuser', newuser);
```

在下面添加一行：

```js
app.use('/adduser', adduser);
```

接下来在`routes`文件夹下面创建`adduser.js`文件，内容如下：

```js
var express = require('express');
var router = express.Router();
// New Code
var mongo = require('mongodb');
var monk = require('monk');
var db = monk('localhost:27017/nodetest1');

/* POST userlist page. */
router.post('/', function (req, res, next) {
    // Get our form values. These rely on the "name" attributes
    var userName = req.body.username;
    var userEmail = req.body.useremail;

    // Set our collection
    var collection = db.get('usercollection');

    // Submit to the DB
    collection.insert({
        "username": userName,
        "email": userEmail
    }, function (err, doc) {
        if (err) {
            // If it failed, return error
            res.send("There was a problem adding the information to the database.");
        } else {
            // If it worked, set the header so the address bar doesn't still say /adduser
            res.location("userlist");
            // And forward to success page
            res.redirect("userlist");
        }
    });

});

module.exports = router;
```

上面的代码是通过post请求拿到表单中的`username`和`email`，然后重定向到`userlist`页面。

启动服务器，在浏览器中打开`http://localhost:3000/newuser`，效果如下所示：

![](http://om1c35wrq.bkt.clouddn.com/newuser.png)

在里面输入用户名和邮箱，就会跳转到`userlist`页面，如下面所示：

![](http://om1c35wrq.bkt.clouddn.com/userlist.png)


现在我们正式的完成了使用Node.js，Exress，Ejs读取和写入Mongodb数据库，我们已经是牛X的程序员了。

恭喜你，真的。如果你认真的看完了这篇教程，并且很认真的学习而不只是复制代码，你应该对routes, views，读数据，写入数据有了完整的概念。这是你用来开发任何其它完整的Web网站所需要的一切知识点！不管你怎么想，我觉得这真挺酷的。