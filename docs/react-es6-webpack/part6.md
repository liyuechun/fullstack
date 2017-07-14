# React and ES6 - Part 6, React and ES6 Workflow with Webpack

这是React和ECMAScript2015系列文章的最后一篇，我们将继续探索React 和 Webpack的使用。

下面是所有系列文章章节的链接：

- [React and ES6 - Part 1, Introduction into ES6 and React](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part1)
- [React and ES6 - Part 2, React Classes and ES7 Property Initializers](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part2)
- [React and ES6 - Part 3, Binding to methods of React class (ES7 included)](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part3)
- [React and ES6 - Part 4, React Mixins when using ES6 and React](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part4)
- [React and ES6 - Part 5, React and ES6 Workflow with JSPM](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part5)
- [React and ES6 - Part 6, React and ES6 Workflow with Webpack](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part6)


> [本篇文章Github源码](https://github.com/yiqizhongchuang/react-es6-es7-gulp-JSPM-Webpack/tree/master/react-es6-es7-gulp-JSPM-Webpack-part6)

|React|JS|
|:----:|:----:|
|![](http://okxh06i2t.bkt.clouddn.com/react.png)|![](http://okxh06i2t.bkt.clouddn.com/js.png)|


## 什么是Webpack?

就像[JSPM](http://yiqizhongchuang.cn/react-and-es6-part-5-react-and-es6-workflow-jspm)一样，`Webpack`是你的前端应用的模块管理的解决方案。

使用[Webpack](https://webpack.github.io/),你能够用一种方便的方法完全控制你的应用资源。

为什么Webpack这么受欢迎？主要有以下几个原因：

- Webpack使用npm作为外部模块源。如果你想添加React到你的项目中，只需要执行 `npm install react`即可。这是一个附加的优势，因为你已经知道如何将你喜欢的库添加到你的项目中。

- 你几乎可以加载所有的东西，而不只是JavaScript。`Webpack`使用名字为`loaders`的装载机来完成加载。这是对应的loaders[清单](https://webpack.github.io/docs/list-of-loaders.html)

- Webpack有一个很强大的开发工具生态系统。像热更新这样的东西将戏剧性的改变你的开发流程。

- 对于各种类型的任务有很多`Webpack plugins`。在大多数情况下，你可以使用已经存在的解决方案。


- Webpack 有很漂亮的logo :)


## Getting started

让我们开始从之前的系列文章中调整我们的应用程序。

首先，我们将要安装初始的开发依赖。

```js
npm install --save-dev webpack
npm install --save-dev babel-core
npm install --save-dev babel-preset-es2015 babel-preset-react babel-preset-stage-0
```

在上面的列表中，`webpack`是自解释型的。`Babel`是用于将ES6转换成ES5(如果你阅读了前面的`React and ES6`系列文章，你应该对ES6和ES5非常熟悉)。自从babel 6后你必须为每一个额外的语言特征安装独立的包。这些包叫做`presets`。我们安装`es2015 preset`,`react preset`和`stage-0 preset`。对于更多关于`babel 6`的信息，你可以阅读[这篇文章](http://egorsmirnov.me/2015/11/03/upgrading-to-babel-6-babelify-and-webpack.html)。

下一步，安装非开发依赖(react和react-dom包)：

`npm install --save react react-dom`

现在在你的项目中基于Webpack最重要的一步。在你的项目根目录下面创建`webpack.config.dev.js`文件。这个文件将用来打包你所有的在一个bundle(或者多个bundle)里面的JavaScript(在大多数项目中不只是JavaScript)，打包完就可以在用户的浏览器中正式运行。

`webpack.config.dev.js`的内容如下：

```js
var path = require('path');
var webpack = require('webpack');

var config = {
    devtool: 'cheap-module-eval-source-map',
    entry: [
        './app.js'
    ],
    output: {
        path: path.join(__dirname, 'dist'),
        filename: 'bundle.js',
        publicPath: '/dist/'
    },
    plugins: [
        new webpack.NoEmitOnErrorsPlugin()
    ]
};

module.exports = config;
```

以上代码的亮点：

- Line 5. 在提高应用程序的各种调试策略中，我们有一个选择，你可以点击[这里](https://webpack.github.io/docs/configuration.html#devtool)了解更多关于`cheap-module-eval-source-map`的内容。

- Lines 6-8. 这里我们定义了`app.js`为应用程序的主入口。

- Lines 9-13. 这个配置制定`Webpack`将打包所有的模块成文件`bundle.js`,并且将`bundle.js`文件放到`dist/`路径下面。

## Webpack loaders
用`Webpack`几乎可以加载所有的东西到你的代码中(这里是[清单](https://webpack.github.io/docs/list-of-loaders.html))。Webpack使用的名字叫做Webpack装载机。

你可以制定文件扩展名关联到特别的装载机。

在我们的案例中，我们将使用`babel-loader`来将`ES2015 / ES6`的代码转换成`ES5`.首先，我们需要安装`npm`依赖包。

```js
npm install --save-dev babel-loader
```

然后，通过添加一些新的装载机关键字到出口对象中来调整`webpack.config.dev.js`文件的配置。

```js
var config = {
    ... add the below code as object key ...
    module: {
        loaders: [
            {
                test: /\.js$/,
                loaders: ['babel-loader'],
                exclude: /node_modules/
            }
        ]
    }
};

module.exports = config;
```
这里需要重点注意的是，我们通过`exclude`关键字的设置禁止`Webpack`解析`node_modules`文件夹里面的文件。

接下来我们在项目的根目录下面添加`.babelrc`文件。

```js
{
  "presets": ["react", "es2015", "stage-0"]
}
```

这个文件是配置`babel`以便能够使用前面我们添加的`react`,`es2015`和`stage-0`presets。

现在，无论什么时候`Webpack`遇到，比如：`import CartItem from './cartItem.js'`,它将加载这个文件并且将`ES6`转换成`ES5`。

## 添加`Webpack`开发服务器

为了运行这个程序，我们需要在服务器上运行这些文件。

幸运的是，`Webpack`生态系统已经提供所有你需要的东西。你可以使用[Webpack开发服务器](https://webpack.github.io/docs/webpack-dev-server.html)或者[Webpack开发中间件，比如：Express.js](https://github.com/webpack/webpack-dev-middleware)。

我们将使用后者。优势是在内存中处理文件时速度快。

让我们安装`npm`模块:

```js
npm install --save-dev webpack-dev-middleware express
```

下一步，在根目录下面添加`server.js`文件：

```js
var path = require('path');
var express = require('express');
var webpack = require('webpack');
var config = require('./webpack.config.dev');

var app = express();
var compiler = webpack(config);

var port = 3000;

app.use(require('webpack-dev-middleware')(compiler, {
    noInfo: true,
    publicPath: config.output.publicPath
}));

app.use(require('webpack-hot-middleware')(compiler));

app.get('*', function (req, res) {
    res.sendFile(path.join(__dirname, 'index.html'));
});

app.listen(port, function onAppListening(err) {
    if (err) {
        console.error(err);
    } else {
        console.info('==> Webpack development server listening on port');
    }
});
```

这是典型的使用`Webpack Dev Middleware`的`express.js`服务器。

## 添加热刷新模块


`Webpack Dev Middleware`已经包含了热刷新的特性。无论什么时候，你的代码发生变化，它都会立即刷新页面。

如果想简单的看看热刷新的演示效果，可以看看`Dan Abramov`的[视频](https://vimeo.com/100010922)。

为了激活`Hot Module Reloading`,你首先得安装必须得`npm`包。

```js
npm install --save-dev webpack-hot-middleware
```

然后在`webpack.config.dev.js`文件中设置`entry`和`plugins`:

```js
var config = {
    entry: [
        './app.js',
        'webpack-hot-middleware/client'
    ],

    ...

    plugins: [
        new webpack.HotModuleReplacementPlugin(),
        new webpack.NoEmitOnErrorsPlugin()
    ]
};

module.exports = config;
```

如果想对React 应用更进一步使用模块刷新其实有很多种方法。

其中一个简单的方法就是安装`babel-preset-react-hmre`模块。

```js
npm install --save-dev babel-preset-react-hmre
```

调整`.babelrc`文件的内容：

```js
{
  "presets": ["react", "es2015", "stage-0"],
  "env": {
    "development": {
      "presets": ["react-hmre"]
    }
  }
}
```

到这一步，这个应用就具备热刷新的功能。

## 最后几步

- 创建`index.html`文件

```js
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>React and ES6 Part 6</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/foundation/5.5.2/css/foundation.min.css">
</head>
<body>
<nav class="top-bar" data-topbar role="navigation">
    <section class="top-bar-section">
        <ul class="left">
            <li class="active">
                <a href="http://egorsmirnov.me/2016/04/11/react-and-es6-part6.html" target="_blank">
                    Blog post at egorsmirnov.me: React and ES6 - Part 6, React and ES6 Workflow with Webpack
                </a>
            </li>
        </ul>
    </section>
</nav>
<div class="root"></div>
<script src="/dist/bundle.js"></script>
</body>
</html>
```


- 创建`app.js`文件

```js
import React from 'react';
import ReactDOM from 'react-dom';
import CartItem from './cartItem.js';

const order = {
    title: 'Fresh fruits package',
    image: 'http://images.all-free-download.com/images/graphiclarge/citrus_fruit_184416.jpg',
    initialQty: 3,
    price: 8
};

ReactDOM.render(
    < CartItem
        title={order.title}
        image={order.image}
        initialQty={order.initialQty}
        price={order.price
        }
    />,
    document.querySelector('.root')
)
;
```

- 创建`cartItem.js`文件

```js
import React from 'react';

export default class CartItem extends React.Component {

    static propTypes = {
        title: React.PropTypes.string.isRequired,
        price: React.PropTypes.number.isRequired,
        initialQty: React.PropTypes.number
    };

    static defaultProps = {
        title: 'Undefined Product',
        price: 100,
        initialQty: 0
    };

    state = {
        qty: this.props.initialQty,
        total: 0
    };

    constructor(props) {
        super(props);
    }

    componentWillMount() {
        this.recalculateTotal();
    }

    increaseQty() {
        this.setState({qty: this.state.qty + 1}, this.recalculateTotal);
    }

    decreaseQty() {
        let newQty = this.state.qty > 0 ? this.state.qty - 1 : 0;
        this.setState({qty: newQty}, this.recalculateTotal);
    }

    recalculateTotal() {
        this.setState({total: this.state.qty * this.props.price});
    }

    render() {
        return (
            <article className="row large-4">
                <figure className="text-center">
                    <p>
                        <img src={this.props.image}/>
                    </p>
                    <figcaption>
                        <h2>{this.props.title}</h2>
                    </figcaption>
                </figure>
                <p className="large-4 column"><strong>Quantity: {this.state.qty}</strong></p>

                <p className="large-4 column">
                    <button onClick={this.increaseQty.bind(this)} className="button success">+</button>
                    <button onClick={this.decreaseQty.bind(this)} className="button alert">-</button>
                </p>

                <p className="large-4 column"><strong>Price per item:</strong> ${this.props.price}</p>

                <h3 className="large-12 column text-center">
                    Total: ${this.state.total}
                </h3>

            </article>
        );
    }
}
```

## 修改`package.json`

现在已经将前面所有碎片化的代码已经整合在一个项目中。

我们需要在`package.json`文件的`scripts`区域添加一些脚本。

```js
{
  "name": "awesome-application",
  "version": "1.0.0",
  ...
  "scripts": {
    "start": "node server.js"
  },
  ...
}
```

## 运行项目
- 运行`npm start`

- 在浏览器中打开`http://localhost:3000`

- 项目运行效果图
[![cover](http://oleey04q4.bkt.clouddn.com/Snip20170217_13.png)](http://oleey04q4.bkt.clouddn.com/Snip20170217_13.png)

## Webpack生产环境配置

现在我们能够在服务器上运行我们的应用程序并且能够通过热模块更新刷新我们的页面。

但是如果我们想要将产品部署到生产环境？没问题，`Webpack`有对应的解决方案。

创建`webpack.config.prod.js`文件，文件内容为:

```js
var path = require('path');
var webpack = require('webpack');

var config = {
    devtool: 'source-map',
    entry: [
        './app.js'
    ],
    output: {
        path: path.join(__dirname, 'dist'),
        filename: 'bundle.js'
    },
    plugins: [
        new webpack.optimize.OccurrenceOrderPlugin(),
        new webpack.DefinePlugin({
            'process.env': {
                'NODE_ENV': JSON.stringify('production')
            }
        }),
        new webpack.optimize.UglifyJsPlugin({
            compressor: {
                warnings: false
            }
        })
    ],
    module: {
        loaders: [
            {
                test: /\.js$/,
                loaders: ['babel-loader'],
                exclude: /node_modules/
            }
        ]
    }
};

module.exports = config;
```

它和开发模式下的配置文件有点相似，但是有以下不同点：

- 热刷新的功能不再有，因为在生产环境中不需要这个功能。

- `JavaScript bundle`被依赖于`webpack.optimize.UglifyJsPlugin`的`UglifyJs`压缩。

- 环境变量`NODE_ENV`被设置成`production`。这需要屏蔽来自React开发环境中的警告。

下一步，更新`package.json`文件中的`scripts`：

```js
{
  ...
  "scripts": {
     "start": "node server.js",
     "clean": "rimraf dist",
     "build:webpack": "NODE_ENV=production webpack --progress --colors --config webpack.config.prod.js",
     "build": "npm run clean && npm run build:webpack"
  },
  ...
}
```

到现在为止，如果你在控制台运行`npm run build`,压缩文件`bundle.js`将被创建并且放在`dist/`路径下面。这个文件准备在生产环境中使用。

## 这只是刚刚开始

我们刚才学到的东西只是`Webpack`的一些基础。

`Webpack`是一个很容易入门的工具，但是要想精通，需要点时间好好研究研究。


## 参考文档

- [Official Webpack website](https://webpack.github.io/)
- [List of Webpack loaders](https://webpack.github.io/docs/list-of-loaders.html)
- [Babel 6 presets](https://babeljs.io/docs/plugins/preset-es2015/)
- [Upgrading to Babel 6. Babel 6 for Babelify and WebPack](http://egorsmirnov.me/2015/11/03/upgrading-to-babel-6-babelify-and-webpack.html)
- [Technical details of Webpack Hot Module Replacement](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html)
- [Video with process of Hot Module Reloading](https://vimeo.com/100010922)
- [About Webpack Dev Server](https://webpack.github.io/docs/webpack-dev-server.html)
- [About Webpack Dev Middleware](https://github.com/webpack/webpack-dev-middleware)
- [About multiple entry points in Webpack](https://webpack.github.io/docs/multiple-entry-points.html)
- [About stylesheets in Webpack](https://webpack.github.io/docs/stylesheets.html)
