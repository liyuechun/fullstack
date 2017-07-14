## 创建 `userlist`路由和视图文件

如下图所示：

![](http://oswrtif8l.bkt.clouddn.com/WechatIMG150.jpeg)

- `userlist.js`内容如下：

```js
var express = require('express');
var router = express.Router();

/**
 * 这几行会告诉app我们需要连接MongoDB，我们用Monk来负责这个连接，我们数据库位置是localhost:27017/nodetest1。注意27017是mongodb的默认端口，如果因为某些原因你修改了端口，记录这里也要跟着改。
 */
var mongo = require('mongodb');
var monk = require('monk');
var db = monk('127.0.0.1:27017/nodetest1');

/* GET userlist page. */
router.get('/', function (req, res, next) {
    var collection = db.get('usercollection');
    collection.find({}, {}, function (e, docs) {
        res.render('userlist', {"userlist": docs});
    });
});

module.exports = router;
```

- `userlist.ejs`内容如下：

```js
<!DOCTYPE html>
<html>

<head>
    <title>USERLIST</title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
</head>

<body>
    <h1>Userlist</h1>
    <ul>

<%
for(var i in userlist){
%>
            <li>
                <a href="mailto:<%=userlist[i].email%>">
                    <%=userlist[i].username%>
                </a>
            </li>
            <% } %>
    </ul>
</body>

</html>
```

保存文件，重启node服务器。希望你还记得怎么重启。打开浏览器，访问`http://localhost:3000/userlist`，你应该能看到这样的界面：

![](http://oswrtif8l.bkt.clouddn.com/WechatIMG151.jpeg)

现在你从数据库里取到了数据并且显示到了网页上。太棒了。