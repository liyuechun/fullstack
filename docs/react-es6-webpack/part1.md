# React and ES6 - Part 1, Introduction

这是React和ECMAScript6结合使用系列文章的第一篇。

下面是所有系列文章章节的链接：

- [React and ES6 - Part 1, Introduction into ES6 and React](http://www.kongyixueyuan.cn/#docs/react-and-es6/part1)
- [React and ES6 - Part 2, React Classes and ES7 Property Initializers](http://www.kongyixueyuan.cn/#docs/react-and-es6/part2)
- [React and ES6 - Part 3, Binding to methods of React class (ES7 included)](http://www.kongyixueyuan.cn/#docs/react-and-es6/part3)
- [React and ES6 - Part 4, React Mixins when using ES6 and React](http://www.kongyixueyuan.cn/#docs/react-and-es6/part4)
- [React and ES6 - Part 5, React and ES6 Workflow with JSPM](http://www.kongyixueyuan.cn/#docs/react-and-es6/part5)
- [React and ES6 - Part 6, React and ES6 Workflow with Webpack](http://www.kongyixueyuan.cn/#docs/react-and-es6/part6)


> [本篇文章Github源码](https://github.com/yiqizhongchuang/react-es6-es7-gulp-JSPM-Webpack/tree/master/react-es6-es7-gulp-JSPM-Webpack-part1)

 [![cover](http://okxh06i2t.bkt.clouddn.com/react.png)](http://okxh06i2t.bkt.clouddn.com/react.png)        [![cover](http://okxh06i2t.bkt.clouddn.com/js.png)](http://okxh06i2t.bkt.clouddn.com/js.png)   

自从ReactJS v0.13.0 Beta 1宣布，React 组建可以使用ECMAScript 6 后，它能给我们带来什么好处呢？

ECMAScript6 (或者 ECMAScript 2015)是一个新的标准(它从26.06.2015更新，在2015年定稿，它是现在官方的标准-[链接](https://esdiscuss.org/topic/ecmascript-2015-is-now-an-ecma-standard)),它为JavaScript带来了很多新的特性，例如：classes，arrow functions, rest parameters, iterators, generators … 等更多的新特性。


如果你不是特别熟悉ECMAScript 2015的新特性，我将重点推荐你看看这个[链接](https://babeljs.io/learn-es2015/)。


看看ES6的[兼容性表](http://kangax.github.io/compat-table/es6/)，我们会发现不是所有的浏览器都支持ES2015的新特性。幸运的是，你不需要花时间等所有的浏览器都支持ES2015，现在已经有相关的解决方案。你可以使用`transpilers`将ES2015的代码转换成ES5。这很类似于在JavaScript中如何将CoffeeScript转换成Coffee code。

[Babel](https://babeljs.io/)是解决方案之一，它真的很神奇的工具。对于作者有太多的感谢。Babel有什么好处呢，Babel支持大量不同的框架，构建系统、测试框架、模板引擎，[看这里](https://babeljs.io/docs/setup/)。


给你一个快速知道babel如何工作的例子。下面就是我们用ECMAScript 6写的例子：

```js
var evenNumbers = numbers.filter((num) => num % 2 === 0);
```

运行babel以后得到下面的代码：

```js
var evenNumbers = numbers.filter(function (num) {
    return num % 2 === 0;
});
```

同样的方法适用于其它ES6代码到ES5代码的转换。

## 开发环境配置

为了让babel有连续的工作流，我们将使用`Gulp`。这是基于nodejs的任务构建工具，可以自动改善繁琐的任务。

1. 明显，你需要安装nodejs，如果你没有安装，需要在你系统上安装nodejs。

2. 下一步，你将需要安装全局的`Gulp`:`npm install --g gulp`。

3. 创建一个空文件夹，切换到这个文件夹中，在终端输入`npm init`初始化你的`package.json`。

4. 运行`npm install --save react react-dom`,这将安装`react`和`react-dom`到你的`node_modules`文件夹并且作为依赖库保存到`package.json`文件中。

5. 运行`npm install --save-dev gulp browserify babelify vinyl-source-stream babel-preset-es2015 babel-preset-react`,这将安装更多的依赖到你的`node_modules`文件夹。


如果想要了解更多关于`step 5`中的内容，请移步：[my article about Browserify, Babelify and ES6](http://egorsmirnov.me/2015/05/25/browserify-babelify-and-es6.html)


## 创建 gulpfile.js

在你的根目录下面创建`gulpfile.js`文件，并将下面的代码拷贝到里面。

```js
var gulp = require('gulp');
var browserify = require('browserify');
var babelify = require('babelify');
var source = require('vinyl-source-stream');

gulp.task('build', function () {
    return browserify({entries: './app.jsx', extensions: ['.jsx'], debug: true})
        .transform('babelify', {presets: ['es2015', 'react']})
        .bundle()
        .pipe(source('bundle.js'))
        .pipe(gulp.dest('dist'));
});

gulp.task('watch', ['build'], function () {
    gulp.watch('*.jsx', ['build']);
});

gulp.task('default', ['watch']);
```

我猜，相关的一些解释将对你有帮助：

- Lines 1-4. 我们需要安装node.js modules并且将他们赋值给变量。

- Line 6. 我们定义gulp任务名为`build`以便我们可以使用`gulp build`来运行。

- Line 7. 我们开始描述我们的构建任务将做什么。我们告知Gulp来使用app.jsx。我们打开调试模式，这有利于我们开发调试。

- Lines 8-11. 我们应用`Babelify`来转换我们的代码。这允许我们将ECMAScript6转换成ECMAScript5。下一步我们将结果输出到dist/bundle.js文件。这几行代码现在使用了Babel 6 中叫做presets的新特性。

- Lines 14-16. 我们定义一个名字叫做`watch`的任务以便我们能够通过`gulp watch`来运行。无论什么时候jsx文件内容发生变化，这个任务都会重新进行编译。

- Line 18. 我们定义默认的gulp任务以便我们输入gulp时自动运行，此任务只执行监视任务。


典型的工作流将是在终端输入`gulp`然后按下`enter`键。它将连续的监听react组建中代码的变化。

## JSX and Babel

你可能已经注意到我们在使用`.jsx`代替`.js`语法。JSX是ReactJS团队研发的JavaScript扩展语法。这个格式对于ReactJS的开发更加方便快捷。

这是关于[JSX](https://facebook.github.io/react/docs/jsx-in-depth.html)更多的信息。

## 使用ECMAScript 6编写第一个React 组建

*这个时候希望你多一点耐心*

在项目的根目录下面，添加一个`hello-world.jsx`文件，并且在文件中code下面的代码。这是我们用ES6编写的第一个非常简单的React组建。

```js
import React from 'react';

class HelloWorld extends React.Component {
    render() {
        return <h1>Hello from {this.props.phrase}!</h1>;
    }
}

export default HelloWorld;
```

代码解析：
- Line 1. 导入React库并且将它存到变量React中。

- Lines 3-8. 使用ES6创建继承自`React.Component`的类.
我们添加非常简单的`render`方法并且`return`一个包含phrase属性的`<h1>`标签。

- Line 9. 使用`export default HelloWorld`将创建的组建导出以便在其它地方能够正常导入使用。

为了便于理解，你也可以将下面ES5代码将上面文件中的代码替换，你会发现下面的代码就是上面代码的变体，上面是ES6，下面是ES5.

```js
import React from 'react';

var HelloWorld = React.createClass({
    render: function() {
        return (
            <h1>Hello from {this.props.phrase}!</h1>
        );
    }
});

export default HelloWorld;
```

## 结束了

让我们来结束我们简单的例子。

创建名字为`app.jsx`的文件。

```js
import HelloWorld from './hello-world';
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
    <HelloWorld phrase="ES6"/>,
    document.querySelector('.root')
);
```
这里我们导入上面创建的组建`HelloWorld`并且将`HelloWorld`创建了对象并且设置`phrase`属性。请注意，这里我们使用特殊的库`ReactDOM`来渲染`HelloWorld`组建。

下一步，我们来创建`index.html`.

```js
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>ReactJS and ES6</title>
</head>
<body>
<div class="root"></div>
<script src="dist/bundle.js"></script>
</body>
</html>
```

现在在终端运行`gulp`命令(它将创建`dist/bundle.js`文件)并且在浏览器中打开index.html，运行效果如下：
[![cover](http://okxh06i2t.bkt.clouddn.com/Snip20170208_24.png)](http://okxh06i2t.bkt.clouddn.com/Snip20170208_24.png)

## 参考文档
- [Classes in ECMAScript 6](http://www.2ality.com/2015/02/es6-classes-final.html)
- [Exploring ES6, e-book](https://leanpub.com/exploring-es6/)
- [BabelJS](https://babeljs.io/)
- [An Introduction to Gulp.js](https://www.sitepoint.com/introduction-gulp-js/)
- [JSX in Depth](https://facebook.github.io/react/docs/jsx-in-depth.html)
