# React and ES6 - Part 5, React and ES6 Workflow with JSPM

这是React和JSPM结合使用系列文章的第五篇。

下面是所有系列文章章节的链接：

- [React and ES6 - Part 1, Introduction into ES6 and React](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part1)
- [React and ES6 - Part 2, React Classes and ES7 Property Initializers](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part2)
- [React and ES6 - Part 3, Binding to methods of React class (ES7 included)](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part3)
- [React and ES6 - Part 4, React Mixins when using ES6 and React](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part4)
- [React and ES6 - Part 5, React and ES6 Workflow with JSPM](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part5)
- [React and ES6 - Part 6, React and ES6 Workflow with Webpack](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part6)


> [本篇文章Github源码](https://github.com/yiqizhongchuang/react-es6-es7-gulp-JSPM-Webpack/tree/master/react-es6-es7-gulp-JSPM-Webpack-part5)

 [![cover](http://okxh06i2t.bkt.clouddn.com/react.png)](http://okxh06i2t.bkt.clouddn.com/react.png)        [![cover](http://okxh06i2t.bkt.clouddn.com/js.png)](http://okxh06i2t.bkt.clouddn.com/js.png)  

## 什么是JSPM？

作为开发者在开始写一个前端项目之前，我们还有很多必须要做的事情。你必须思考用什么JavaScript模块装载系统，gulp / grunt / npm ？你必须选择CSS预处理工具，
你必须思考源码集成和其它很多事情。


倘若我们不想有太多的麻烦而是想要在15分钟之内开始编码。我们只需要提供一个工具即可。

[JSPM(JavaScript包管理器)](http://jspm.io/)就是其中的工具之一。

- `JSPM`允许你用不同的形式(ES6, CommonJS, AMD and global)加载JavaScript模块。

- 它运行你从`npm or github`注册安装相关依赖。

- JSPM支持ES6+以上的语法

- 你可以毫无麻烦的加载CSS／图片／字体和其它格式的文件，在一些插件的帮助下，这完全是可能的(本文稍微叙述)。

- JSPM让为生产研发的使用准备相关文件变得更容易。

在这篇文章中，我们将重新创建和之前一样的新的项目，但是我们使用`JSPM`。

给你推荐`JSPM wiki`,你可以从这里了解更多。

## JSPM + React 项目初始化

首先执行下面的命令：

`npm install jspm -g`

JSPM将作为一个全局的npm模块被安装。安装完毕以后，在我们系统中就可以使用`jspm`这个命令。

下一步，切换到你的项目路径下面执行`jspm init`命令，一直按`enter`按钮按照默认值安装即可，安装过程截图如下：

[![cover](http://oleey04q4.bkt.clouddn.com/Snip20170216_10.png)](http://oleey04q4.bkt.clouddn.com/Snip20170216_10.png)

安装过程中，你需要等待一会儿，JSPM需要下载一些初始的依赖，并且会将这些依赖放到一个文件夹名字为`jspm_packages`的文件中。

通过`JSPM`使用`jspm_packages`文件夹就像`jQuery / React / AngularJS etc.`存储和管理相关的包，最终将会被应用到你的项目中，就像`node_modules`或者`bower_components`一样。

`jspm_packages`有一个比较大的不同。`JSPM`可以从很多地方(Github / npm / bower)来存储相关依赖，非常赞的是，你在的代码里面，不管依赖包来自`npm`还是`Github repository`，你可以可以同等对待。


## 安装必须的依赖包

运行完`jspm init`的将要产生的下一个文件是`config.js`。


首先，`config.js`提供我们安装的依赖的版本配置。除此以外，你还可以对相关包重命名你喜欢的名字。


Let’s understand what I mean by all that. Run this command:

我们理解上面的内容以后，接下来，执行下面的命令：

`jspm install npm:jspm-loader-jsx`

If you’ll take a look at config.js now you should see excerpt like below:

接下来，你看看`config.js`文件，你将发现增加了下面这句代码:

```js
map: {
    ...
    "jspm-loader-jsx": "npm:jspm-loader-jsx@0.0.7"
    ...
}
```

现在在我们的代码中，我们将有可能使用这个模块中的`jspm-loader-jsx`.让我们来看看，如果我们不喜欢这个名字，我们如何将`jspm-loader-jsx`名字替换成`jsx`。

执行下面的命令：

```js
jspm uninstall jspm-loader-jsx
jspm install jsx=npm:jspm-loader-jsx
```

我的插件将关联到`jsx`，并且你可以在你的代码中通过`import jsx`将其导入到你的代码中。顺便说一下，我们刚安装的这个包是我们的项目所需的包。所以，不要删除`jspm-loader-jsx`。

让我们安装其它我们将要使用的依赖包：

`jspm install react react-dom`

在这里我们没有使用`npm`或者`github`前缀，因为`JSPM`有常用包的[注册表](https://github.com/jspm/registry)。你也能够添加一个包到这个注册表中。

如果你想研究更深，你可以阅读[这篇文章](https://github.com/jspm/jspm-cli/blob/master/docs/installing-packages.md)。

## config.js

Inside config.js find key named babelOptions and replace it with following lines:



在`config.js`文件中找到关键字`babelOptions`的地方，然后替换成下面的代码：

```js
babelOptions: {
    "stage": 0,
    "optional": [
      "runtime",
      "optimisation.modules.system"
    ]
}
```

这是需要让Babel和类型属性一样支持所有的新特性。

## 完成项目结构

该是写代码的时候了。

创建一个名字为`app.jsx`的文件，从[这里](https://github.com/yiqizhongchuang/react-es6-es7-gulp-JSPM-Webpack/blob/master/react-es6-es7-gulp-JSPM-Webpack-part5/app.jsx)拷贝代码到`app.jsx`中。下一步创建`cartItem.jsx`文件，将[这里](https://github.com/yiqizhongchuang/react-es6-es7-gulp-JSPM-Webpack/blob/master/react-es6-es7-gulp-JSPM-Webpack-part5/cartItem.jsx)的内容拷贝到里面。我希望你对这里的内容所有了解，如果不了解，可以看前面的几篇文章。

创建`index.html`文件，并且从[这里](https://github.com/yiqizhongchuang/react-es6-es7-gulp-JSPM-Webpack/blob/master/react-es6-es7-gulp-JSPM-Webpack-part5/index.html)拷贝内容到文件中，我相信你会对下面的代码感兴趣。

```js
<script src="jspm_packages/system.js"></script>
<script src="config.js"></script>
<script>
    System.import('app.jsx!');
</script>
```

在`JSPM`中通过一个叫做[SystemJS](https://github.com/systemjs/systemjs)的库来实现模块动态加载已成为可能。这个库的作者是`JSPM`和`SystemJS`的作者，名叫叫做`Guy Bedford`。

在第一行，我们导入`SystemJS`，在第二行，我们导入`config.js`，`SystemJS`和`config.js`都是通过`JSPM`创建。通过`System.import('app.jsx!')`,我们添加入口文件`app.jsx`，然后再通过这个入口加载其它所有的东西。

到这里我们就基本完成了，现在在你的浏览器中打开index.html。你将看到下面的效果图：

[![cover](http://oleey04q4.bkt.clouddn.com/Snip20170216_11.png)](http://oleey04q4.bkt.clouddn.com/Snip20170216_11.png)

注意： `!`添加在`app.jsx`的后面，这是一个`JSPM`加载非JavaScript文件的约定。

## 其它的JSPM特性

在我们这篇博客中并没有包含所有的JSPM的特性。

下面是我想给大家突出的比较显著的一些特点。

## 通用的JavaScript模块的加载

`JSPM`支持不同格式的模块。所以在同一个文件中使用ES6来加载一些`require.js`模块是绝对合法的。

在JSPM中如果想了解更多关于模块加载，请看[这里](https://github.com/systemjs/systemjs/blob/master/docs/module-formats.md)。

## 加载文本文件，CSS和其它资源

例如：执行下面的命令

```js
jspm install css
```

导入到某个文件中：

```js
import './some/style.css!';
```

阅读[更多关于JSPM插件的信息](https://github.com/jspm/jspm-cli/blob/master/docs/plugins.md)

## 生产环境构建
使用其它一些工具你可以连接和减少一些源代码。

Run the following command from application directory:

在你的项目文件夹下面执行下面的命令：

```js
jspm bundle-sfx app.jsx! app.js --skip-source-maps --minify
```

你将有一个单一的很小的包含模块和依赖裤的文件。


想了解更多关于生产环境的配置，请[点击这里](https://github.com/jspm/jspm-cli/blob/master/docs/production-workflows.md).


## 参考文档

- [Official JSPM website](http://jspm.io/)
- [JSPM wiki](https://github.com/jspm/jspm-cli/wiki)
- [SystemJS wiki](https://github.com/systemjs/systemjs)
- [SystemJS Module Loaders](https://github.com/systemjs/systemjs/blob/master/docs/module-formats.md)
- [JSPM plugins](https://github.com/jspm/jspm-cli/blob/master/docs/plugins.md)
- [JSPM production workflow](https://github.com/jspm/jspm-cli/blob/master/docs/production-workflows.md)
