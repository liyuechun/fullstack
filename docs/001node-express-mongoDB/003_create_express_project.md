## 创建一个Express项目

我们准备使用`Express`和`Ejs`，这不是用来做CSS预处理的。我们会手写一些CSS。我们要用Ejs或者其它的模板引擎来处理Node和Express的数据。如果你会HTML的话，Ejs并不难。只要记住你需要集中精神，否则很容易出错。

```js
liyuechun:Desktop yuechunli$ pwd
/Users/liyuechun/Desktop
liyuechun:Desktop yuechunli$ mkdir 60MINUTES
liyuechun:Desktop yuechunli$ cd 60MINUTES/
bogon:60MINUTES yuechunli$ express -e nodetest1

  warning: option `--ejs' has been renamed to `--view=ejs'


   create : nodetest1
   create : nodetest1/package.json
   create : nodetest1/app.js
   create : nodetest1/public
   create : nodetest1/views
   create : nodetest1/views/index.ejs
   create : nodetest1/views/error.ejs
   create : nodetest1/routes
   create : nodetest1/routes/index.js
   create : nodetest1/routes/users.js
   create : nodetest1/bin
   create : nodetest1/bin/www
   create : nodetest1/public/javascripts
   create : nodetest1/public/stylesheets
   create : nodetest1/public/stylesheets/style.css

   install dependencies:
     $ cd nodetest1 && npm install

   run the app:
     $ DEBUG=nodetest1:* npm start

   create : nodetest1/public/images
bogon:60MINUTES yuechunli$ 
```

VSCode打开，项目结构如下：

![](http://oswrtif8l.bkt.clouddn.com/WechatIMG140.jpeg)