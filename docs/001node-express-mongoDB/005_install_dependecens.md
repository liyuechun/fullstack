## 安装依赖

现在我们定义好了项目的依赖项。*号会告诉NPM“安装最新版本”。回到命令行窗口，进入nodetest1目录，输入：

```js
bogon:nodetest1 yuechunli$ ls
app.js          package.json    routes
bin             public          views
bogon:nodetest1 yuechunli$ npm install
npm notice created a lockfile as package-lock.json. You should commit this file.
added 80 packages in 4.563s
bogon:nodetest1 yuechunli$ npm start

> nodetest1@0.0.0 start /Users/liyuechun/Desktop/60MINUTES/nodetest1
> node ./bin/www

Express server listening on port 3000...
GET / 200 13.288 ms - 207
GET /stylesheets/style.css 200 3.295 ms - 111
GET /favicon.ico 404 1.757 ms - 1136
```

浏览器输入`http://127.0.0.1:3000`,效果图如下：

![](http://oswrtif8l.bkt.clouddn.com/WechatIMG141.jpeg)
