# React and ES6 - Part 3, Binding to methods of React class (ES7 included)

这是React和ECMAScript6／ECMAScript7结合使用系列文章的第三篇。

下面是所有系列文章章节的链接：

- [React and ES6 - Part 1, Introduction into ES6 and React](http://www.kongyixueyuan.cn/#docs/react-and-es6-webpack/part1)
- [React and ES6 - Part 2, React Classes and ES7 Property Initializers](http://www.kongyixueyuan.cn/#docs/react-and-es6-webpack/part2)
- [React and ES6 - Part 3, Binding to methods of React class (ES7 included)](http://www.kongyixueyuan.cn/#docs/react-and-es6-webpack/part3)
- [React and ES6 - Part 4, React Mixins when using ES6 and React](http://www.kongyixueyuan.cn/#docs/react-and-es6-webpack/part4)
- [React and ES6 - Part 5, React and ES6 Workflow with JSPM](http://www.kongyixueyuan.cn/#docs/react-and-es6-webpack/part5)
- [React and ES6 - Part 6, React and ES6 Workflow with Webpack](http://www.kongyixueyuan.cn/#docs/react-and-es6-webpack/part6)


> [本篇文章Github源码](https://github.com/yiqizhongchuang/react-es6-es7-gulp-JSPM-Webpack/tree/master/react-es6-es7-gulp-JSPM-Webpack-part3)

 [![cover](http://okxh06i2t.bkt.clouddn.com/react.png)](http://okxh06i2t.bkt.clouddn.com/react.png)        [![cover](http://okxh06i2t.bkt.clouddn.com/js.png)](http://okxh06i2t.bkt.clouddn.com/js.png)   


你在看[上一篇文章](http://yiqizhongchuang.cn/react-and-es6-part-2-react-classes-and-es7-property-initializers)中的`CartItem render 方法`中使用`{this.increaseQty.bind(this)}`代码时，你肯定会觉得困惑。


如果我们尝试将`{this.increaseQty.bind(this)}`代码替换成`{this.increaseQty}`,我们将在浏览器控制台中发现如下错误：`Uncaught TypeError: Cannot read property 'setState' of undefined`.

[![cover](http://oleey04q4.bkt.clouddn.com/Snip20170215_7.png)](http://oleey04q4.bkt.clouddn.com/Snip20170215_7.png)


这是因为我们当我们调用一个用那种方法绑定的方法时，`this`不是类它本身，`this`未定义。相反，如果在案例中，`React.createClass()`所有的方法将会自动绑定对象的实例。这可能是一些开发者的直觉。


No autobinding was the decision of React team when they implemented support of ES6 classes for React components. You could find out more about reasons for doing so in this blog post.

当`React team`通过`ES6`来实现对React 组建的支持时，没有设置自动绑定是`React team`的一个决定。在[这篇博客](https://facebook.github.io/react/blog/2015/01/27/react-v0.13.0-beta-1.html#autobinding)中你能发现更多关于为什么这么处理的原因。

现在让我们来看看在你的JSX案例中，使用ES6类创建的方法的多种调用方法。我所考虑到的多种不同的方法都在[这个GitHub repository](https://github.com/yiqizhongchuang/react-es6-es7-gulp-JSPM-Webpack/tree/master/react-es6-es7-gulp-JSPM-Webpack-part3)。

## Method 1. 使用Function.prototype.bind().

之前我们已经见到过：

```javascript
export default class CartItem extends React.Component {
    render() {
        <button onClick={this.increaseQty.bind(this)} className="button success">+</button>
    }
}
```

当任何ES6的常规方法在方法原型中进行`this`绑定时，this指向的是类实例。如果你想了解更多`Function.prototype.bind()`,你可以访问[这篇MDN博客](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)

## Method 2. 在构造函数中定义函数

此方法是上一个与类构造函数的用法的混合：

```js
export default class CartItem extends React.Component {

    constructor(props) {
        super(props);
        this.increaseQty = this.increaseQty.bind(this);
    }

    render() {
        <button onClick={this.increaseQty} className="button success">+</button>
    }
}
```

你不要再次在JSX代码的其它地方进行绑定，但是这将增加构造函数中的代码量。

## Method 3. 使用箭头函数和构造函数

[ES6 fat arrow functions](https://babeljs.io/learn-es2015/) preserve this context when they are called. We could use this feature and redefine increaseQty() inside constructor in the following way:


当方法被调用时，[ES6 fat arrow functions](https://babeljs.io/learn-es2015/)会保留上下文。我们能使用这个特征用下面的方法在构造函数中重定义`increaseQty()`函数。

```js
export default class CartItem extends React.Component {

    constructor(props) {
        super(props);
        this._increaseQty = () => this.increaseQty();
    }

    render() {
        <button onClick={_this.increaseQty} className="button success">+</button>
    }
}
```

## Method 4. 使用箭头函数和ES2015+类属性

另外，你可以使用ES2015+类属性语法和箭头函数：

```js
export default class CartItem extends React.Component {

    increaseQty = () => this.increaseQty();

    render() {
        <button onClick={this.increaseQty} className="button success">+</button>
    }
}
```

属性初始化和Method 3中的功能一样。

*警告:* 类属性还不是当前JavaScript标准的一部分。但是你可以在Babel中自由的使用他们。你可以在[这篇文章](https://babeljs.io/docs/usage/experimental/)中了解更多类属性的使用。

## Method 5. 使用 ES2015+ 函数绑定语法.

最近，Babel介绍了`Function.prototype.bind()`中`::`的使用。在这里我就不深入细节讲解它工作的原理。有很多其它的小伙伴已经有很完美的解释。你可以Google搜索`blog 2015 function bind`了解更多细节。

下面是`ES2015+`函数绑定的使用：

```js
export default class CartItem extends React.Component {

    constructor(props) {
        super(props);
        this.increaseQty = ::this.increaseQty;
        // line above is an equivalent to this.increaseQty = this.increaseQty.bind(this);
    }

    render() {
        <button onClick={this.increaseQty} className="button success">+</button>
    }
}
```

强调：这个特性是高度的实验，使用它你得自己承担风险。

## Method 6. 在调用方法的方使用 ES2015+ 函数绑定语法.

You also have a possibility to use ES2015+ function bind syntax directly in your JSX without touching constructor. It will look like:

你也可以直接在非构造函数里面的JSX里面直接使用ES2015+函数绑定。效果如下：

```js
export default class CartItem extends React.Component {
    render() {
        <button onClick={::this.increaseQty} className="button success">+</button>
    }
}
```

非常简洁，唯一的缺点是，这个函数将在每个后续渲染组件上重新创建。这不是最佳的。更重要的是，如果你使用像PureRenderMixin的东西将会出现。

## 结论

在这篇文章中，我们已经发现了多种React组建类方法的绑定方法。

## 参考文档

- [About autobinding in official React blog](https://facebook.github.io/react/blog/2015/01/27/react-v0.13.0-beta-1.html#autobinding)
- [Autobinding, React and ES6 Classes](https://www.ian-thomas.net/autobinding-react-and-es6-classes/)
- [Function Bind Syntax in official Babel blog](http://babeljs.io/blog/2015/05/14/function-bind)
- [Function.prototype.bind()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
- [Experimental ES7 Class Properties](https://gist.github.com/jeffmo/054df782c05639da2adb)
- [Experimental ES7 Function Bind Syntax](https://github.com/tc39/proposal-bind-operator)
