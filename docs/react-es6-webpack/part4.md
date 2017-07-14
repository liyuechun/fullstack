# React and ES6 - Part 4, React Mixins when using ES6 and React

这是React和ECMAScript6／ECMAScript7结合使用系列文章的第四篇。

下面是所有系列文章章节的链接：

- [React and ES6 - Part 1, Introduction into ES6 and React](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part1)
- [React and ES6 - Part 2, React Classes and ES7 Property Initializers](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part2)
- [React and ES6 - Part 3, Binding to methods of React class (ES7 included)](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part3)
- [React and ES6 - Part 4, React Mixins when using ES6 and React](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part4)
- [React and ES6 - Part 5, React and ES6 Workflow with JSPM](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part5)
- [React and ES6 - Part 6, React and ES6 Workflow with Webpack](http://www.kongyixueyuan.cn/#docs/react-es6-webpack/part6)


> [本篇文章Github源码](https://github.com/yiqizhongchuang/react-es6-es7-gulp-JSPM-Webpack/tree/master/react-es6-es7-gulp-JSPM-Webpack-part4)

|React|JS|
|:----:|:----:|
|![](http://okxh06i2t.bkt.clouddn.com/react.png)|![](http://okxh06i2t.bkt.clouddn.com/js.png)|


当我们使用`React.createClass()`时，你有可能使用所谓的混合开发。它们允许插入一些额外的方法到React组建中。这个概念不只是针对React，它也存在于`vanilla JS`和其它的`语言/框架中`。

在ES6的React组建创建中，不太可能使用混合机制。我不会深入考虑这个决定的原因。如果你感兴趣，你可以点击下面的链接，了解更多.

- [About mixins in ES6 in official React blog](https://facebook.github.io/react/blog/2015/01/27/react-v0.13.0-beta-1.html#mixins)

- [Mixins Are Dead. Long Live Composition by Dan Abramov](https://medium.com/@dan_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750)


相反，我们将更加专注于具体的例子。

## Higher-Order Components instead of Mixins

我们将使用本系列文章中的part2中的 `CartItem`组建。你可以从[这里](https://github.com/yiqizhongchuang/react-es6-es7-gulp-JSPM-Webpack/blob/master/react-es6-es7-gulp-JSPM-Webpack-part2/part2-es6/cartItem.jsx)获取到源码。

让我们来看看如何伴随其它的组建生成一个每秒加1的定时器。

为了更好的说明，我们将不改变`CartItem`的代码。我们重新提供新的组建，这个新的组建不会破坏`CartItem`的原始内容。而是会在保持`CartItem`原始内容的情况下，对`CartItem`增强一些额外的方法。

这听起来很神秘，跟着我，他会慢慢的变得更加清晰。

让我们想象我们已经有了`IntervalEnhance`组建：

```js
import React from 'react';
import { IntervalEnhance } from "./intervalEnhance";

class CartItem extends React.Component {
    // component code here
}

export default IntervalEnhance(CartItem);
```

现在该是时候写`IntervalEnhance`组建了。我们将增加一个文件名为`intervalEnhance.jsx`文件。

```js
import React from 'react';

export var IntervalEnhance = ComposedComponent => class extends React.Component {

    static displayName = 'ComponentEnhancedWithIntervalHOC';

    constructor(props) {
        super(props);
        this.state = {
            seconds: 0
        };
    }

    componentDidMount() {
        this.interval = setInterval(this.tick.bind(this), 1000);
    }

    componentWillUnmount() {
        clearInterval(this.interval);
    }

    tick() {
        this.setState({
            seconds: this.state.seconds + 1000
        });
    }

    render() {
        return <ComposedComponent {...this.props} {...this.state} />;
    }
};
```

代码解释：
- Line 3.`ComposedComponent => class extends React.Component`部分的代码等价于定义一个返回class的方法。`ComposedComponent`是我们希望增强的组建(在我们这个案例中，它是`CartItem`),通过使用`export var IntervalEnhance`，我们可以把整个方法作为`IntervalEnhance(指向上面代码中的CartItem)`导出。

- Line 5.帮助调试。在`React DevTools`中，这个组建将被命名为`ComponentEnhancedWithIntervalHOC`。

- Lines 7-12. 初始化一个值为0，名字为`seconds`的状态机变量。

- Lines 14-26. 为这个组建提供启动和停止的生命周期函数。

- Line 29. 在这里有很多感兴趣的东西。这一行将给我的组建增加所有的`state`和`props`并且转换成`CartItem`组建。同时，通过这行代码的设置，在`CartItem`中我们将可以正常访问`this.state.seconds`属性。

最后一步需要改变`CartItem component`的render方法。我们将在这个视图上直接输出`this.state.seconds`.

```js
import React from 'react';
import { IntervalEnhance } from "./intervalEnhance";

class CartItem extends React.Component {
    // component code here

    render() {
        return <article className="row large-4">
                <!-- some other tags here -->                    
                <p className="large-12 column">
                    <strong>Time elapsed for interval: </strong>
                    {this.props.seconds} ms
                </p>    
            </article>;
        }
}
export default IntervalEnhance(CartItem);
```

在浏览器中打开这个页面，你将看见每秒跳动一次的label。

[![cover](http://oleey04q4.bkt.clouddn.com/Snip20170215_8.png)](http://oleey04q4.bkt.clouddn.com/Snip20170215_8.png)


注意 - 没有触碰`CartItem`组建(通过render方法的分离)就全完全搞定！这就是高阶组建的力量。


现在，请看看最后所有的[代码](https://github.com/yiqizhongchuang/react-es6-es7-gulp-JSPM-Webpack/tree/master/react-es6-es7-gulp-JSPM-Webpack-part4/es6)。如果代码不清晰或者有问题，很高兴你能提供相应的反馈。


## Use ES7 Decorators instead of Mixins

如果你喜欢ES7装饰，可以使用一种熟悉的方式来使用他们。

首先，安装`babel-plugin-transform-decorators-legacy`.

```js
npm install --save-dev babel-plugin-transform-decorators-legacy
```

从`GitHub repository`获取修改过的[gulpfile.js](https://github.com/yiqizhongchuang/react-es6-es7-gulp-JSPM-Webpack/blob/master/react-es6-es7-gulp-JSPM-Webpack-part4/es7/gulpfile.js)文件源码。

然后：

```js
import React from 'react';
import { IntervalEnhance } from "./intervalEnhance";

@IntervalEnhance
export default class CartItem extends React.Component {
    // component code here
}
```

## What about PureRenderMixin?

如果你喜欢使用像`PureRenderMixin`的`mixins`,那么在ES6中有不同的方法可以实现相同的功能。他们中的其中一个如下：

```js
class Foo extends React.Component {
   constructor(props) {
     super(props);
     this.shouldComponentUpdate = React.addons.PureRenderMixin.shouldComponentUpdate.bind(this);
   }
   render () {
     return <div>Helllo</div>
   }
 }
```

## 结论
高阶组建功能强大和极具表现力。现在高阶组建广泛的被使用来替代老式的`mixin`句法。随时创建自己的机制来处理组件之间的混合功能。


## 参考文档
- [About mixins in ES6 in official React blog](https://facebook.github.io/react/blog/2015/01/27/react-v0.13.0-beta-1.html#mixins)
- [Mixins Are Dead. Long Live Composition by Dan Abramov](https://medium.com/@dan_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750)
- [Original gist by Sebastian Markbåge showing usage of composition for React components](https://gist.github.com/sebmarkbage/ef0bf1f338a7182b6775)
- [JSX Spread Attributes](https://facebook.github.io/react/docs/jsx-in-depth.html)
- [Gist with PureRenderMixin in ES6](https://gist.github.com/ryanflorence/a93fd88d93cbf42d4d24)
