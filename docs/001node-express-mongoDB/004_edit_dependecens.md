## 编辑依赖项

好了，我们现在有一些基本项目结构，但是还没完。你会注意到express的安装过程在你的nodetest1目录里创建了一个叫`package.json`的文件，用文本编辑器打开这个文件，它应该是长这样的:

```js
{
  "name": "nodetest1",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "start": "node ./bin/www"
  },
  "dependencies": {
    "body-parser": "~1.17.1",
    "cookie-parser": "~1.4.3",
    "debug": "~2.6.3",
    "ejs": "~2.5.6",
    "express": "~4.15.2",
    "morgan": "~1.8.1",
    "serve-favicon": "~2.4.2"
  }
}
```

这是一个标准的JSON格式文件，表明了我们应用的依赖。我们需要添加一点东西。比如对mongodb和Monk的调用。把dependencies部分改成这样：

```js
{
  "name": "nodetest1",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "start": "node ./bin/www"
  },
  "dependencies": {
    "body-parser": "~1.17.1",
    "cookie-parser": "~1.4.3",
    "debug": "~2.6.3",
    "ejs": "~2.5.6",
    "express": "~4.15.2",
    "morgan": "~1.8.1",
    "serve-favicon": "~2.4.2",
    "mongodb": "*",
    "monk": "*"
  }
}
```