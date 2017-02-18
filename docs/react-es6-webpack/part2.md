# React and ES6 - Part 2, React Classes and ES7 Property Initializers

这是React和ECMAScript6结合使用系列文章的第二篇。

下面是所有系列文章章节的链接：

- [React and ES6 - Part 1, Introduction into ES6 and React](http://www.kongyixueyuan.cn/#docs/react-and-es6/part1)
- [React and ES6 - Part 2, React Classes and ES7 Property Initializers](http://www.kongyixueyuan.cn/#docs/react-and-es6/part2)
- [React and ES6 - Part 3, Binding to methods of React class (ES7 included)](http://www.kongyixueyuan.cn/#docs/react-and-es6/part3)
- [React and ES6 - Part 4, React Mixins when using ES6 and React](http://www.kongyixueyuan.cn/#docs/react-and-es6/part4)
- [React and ES6 - Part 5, React and ES6 Workflow with JSPM](http://www.kongyixueyuan.cn/#docs/react-and-es6/part5)
- [React and ES6 - Part 6, React and ES6 Workflow with Webpack](http://www.kongyixueyuan.cn/#docs/react-and-es6/part6)


> [本篇文章Github源码](https://github.com/yiqizhongchuang/react-es6-es7-gulp-JSPM-Webpack/tree/master/react-es6-es7-gulp-JSPM-Webpack-part2)

[![cover](http://okxh06i2t.bkt.clouddn.com/react.png)](http://okxh06i2t.bkt.clouddn.com/react.png)        [![cover](http://okxh06i2t.bkt.clouddn.com/js.png)](http://okxh06i2t.bkt.clouddn.com/js.png)


在[第一篇](http://yiqizhongchuang.cn/react-and-es6-part-1-introduction)文章中，我们开始介绍如何使用ES6来创建静态的组建并且输出`Hello from ES6`. Not so exciting :)

在这篇文章中，我们将创建一个名字叫做`CartItem`的更复杂的组建。它将输出购物车中的一些产品信息，包括图片、标题和价格。

此外，用户可以和`CartItem`组建交互，通过点击`增加`或者`减少`改变items的数量。并且我们的组建将对交互后的总价格做出动态改变。

*最后的项目效果图:*

[![cover](http://okxh06i2t.bkt.clouddn.com/screenshot.png)](http://okxh06i2t.bkt.clouddn.com/screenshot.png)

## 创建index.html文件

让我们来创建一个简单的html模版文件。

```js
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>React and ES6 Part 2</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/foundation/5.5.2/css/foundation.min.css">
</head>
<body>
<div class="root"></div>
<script src="dist/bundle.js"></script>
</body>
</html>
```

注意我们已经通过`cdn`添加了[Foundation CSS](http://foundation.zurb.com/)框架服务.它可以让我们微应用看起来很漂亮。同时，class为root的div将是我们应用加载的地方。

## Gulpfile.js
创建`gulpfile.js`文件，拷贝下面的代码到`gulpfile.js`文件中。

```js
var gulp = require('gulp');
var browserify = require('browserify');
var babelify = require('babelify');
var source = require('vinyl-source-stream');

gulp.task('build', function () {
    return browserify({entries: './app.jsx', extensions: ['.jsx'], debug: true})
        .transform('babelify', {presets: ['es2015', 'react']})
        .bundle()
        .on('error', function(err) { console.error(err); this.emit('end'); })
        .pipe(source('bundle.js'))
        .pipe(gulp.dest('dist'));
});

gulp.task('watch', ['build'], function () {
    gulp.watch('*.jsx', ['build']);
});

gulp.task('default', ['watch']);

```

## package.json
1. 创建一个空文件夹，切换到这个文件夹中，在终端输入`npm init`初始化你的`package.json`。

2. 运行`npm install --save react react-dom`,这将安装`react`和`react-dom`到你的`node_modules`文件夹并且作为依赖库保存到`package.json`文件中。

3. 运行`npm install --save-dev gulp browserify babelify vinyl-source-stream babel-preset-es2015 babel-preset-react`,这将安装更多的依赖到你的`node_modules`文件夹。


## Main application React Component

创建`app.jsx`:

```js
import React from 'react';
import ReactDOM from 'react-dom';
import CartItem from './cartItem';

const order = {
    title: 'Fresh fruits package',
    image: 'http://images.all-free-download.com/images/graphiclarge/citrus_fruit_184416.jpg',
    initialQty: 3,
    price: 8
};

ReactDOM.render(
    <CartItem title={order.title}
              image={order.image}
              initialQty={order.initialQty}
              price={order.price}/>,
    document.querySelector('.root')
);
```

上面的代码做了什么：

- Lines 1-2.  我们导入 React / ReactDOM 库。

- Line 3. 导入我们马上要创建的`CartItem`组建。

- Lines 5-10. 给`CartItem`组建设置相关属性(属性包括 item title, image, initial quantity and price).

- Lines 12-18. 加载CartItem组建到一个CSS类为root的DOM元素上。

## CartItem React Component 架构

现在是创建负责显示项目的数据以及与用户的交互组件的时候了。

添加下面的代码到`cartItem.jsx`文件中：

```js
import React from 'react';

export default class CartItem extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            qty: props.initialQty,
            total: 0
        };
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
}
```

代码解释：

- Lines 4-10. 这是ES6中React类的构造函数。super(props)这句代码是先处理父类的props，这句代码必不可少。下面的状态机变量的设置相当于ES5中`getInitialState()`方法状态机变量的初始化，我们通过`this.state`来给状态机变量设置初始值。个人意见，我比较喜欢ES6中构造函数的写法。

- Lines 11-13. `componentWillMount()`是生命周期中的方法，在这个方法里面我们通过`recalculateTotal()`方法对总价格做了计算。

- Lines 14-20. 给用户提供增加和减少的交互方法。当用户点击相应的按钮(如前面的效果图所示)时，这两个方法会被调用。


## CartItem render method

在`CartItem`类中添加新的方法：

```js
export default class CartItem extends React.Component {

    // previous code we wrote here

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
        )
    }
}
```

这里我们只是使用JSX+组建，再加上一些基础的CSS输出漂亮的标签。

不要担心`{this.increaseQty.bind(this)}`这句代码的使用，下一小结中我们将会详细讲解，现在你需要相信我，这句代码会调用`CartItem`类中的`increaseQty()`方法即可。

因此，到现在我们已经有了和用户交互的漂亮的应用了，它向我们展示了如何使用`ECMAScript6`来编写React 组建。就我个人而言，我很喜欢ES6带来的新的语法。

到现在我们还没有完成。在完成这篇文章之前，我们将再看看ES6中其它的一些新的特性。


## Default Props and Prop Types for ES6 React classes

想象我们想要为CartItem组建添加一些验证和默认值。

幸运的是，你只需要在`CartItem`类中添加如下代码即可。

```js
static get defaultProps() {
    return {
      title: 'Undefined Product',
      price: 100,
      initialQty: 0
    }
}


static propTypes = {
  title: React.PropTypes.string.isRequired,
  price: React.PropTypes.number.isRequired,
  initialQty: React.PropTypes.number
}
```

PS: 也可以将上面的代码删除，在`cartItem`里面非`CartItem`类的内部添加如下代码：

```js
CartItem.defaultProps = {
    title: 'Undefined Product',
    price: 100,
    initialQty: 0
}

CartItem.propTypes = {
    title: React.PropTypes.string.isRequired,
    price: React.PropTypes.number.isRequired,
    initialQty: React.PropTypes.number
}

```

就我个人而言，比较喜欢第一种写法，它不会破坏类的封装性。

添加上面的代码后，如果你给`title`属性添加`numeric`类型的值，将在控制台出现警告，这就是React属性验证的功能。

## Bringing ES7 into the project

你可能会问一个合理的问题，为什么ES6还没有定稿，在你的标题中为什么出现ES7呢？

我会告诉你，让我们展望未来。我们开始使用non-breaking、property initializers和decorators的新特性。

即使ES7还处于非常早期的阶段，在Babel中已经实现了部分的建议性的新特性。这些实验性的新特性是从ES5到ES7令人惊叹的新的特性的转变。

首先，通过`npm install --save-dev babel-preset-stage-0`命令安装缺失的`npm module`。

其次，为了在我们项目中能够使用新的语法，我们需要在`gulpfile.js`文件的第`8`行做一些改变，代码如下：

```js
.transform('babelify', {presets: ['react', 'es2015', 'stage-0']})
```
你可以从[GitHub repository](https://github.com/yiqizhongchuang/react-es6-es7-gulp-JSPM-Webpack/blob/master/react-es6-es7-gulp-JSPM-Webpack-part2/part2-es6/gulpfile.js)直接拷贝`gulpfile.js`完整的代码。

## ES7 React 组建属性初始化／默认值／类型

Inside  class add this right above :


在`CartItem`中将下面的代码：

```js
static get defaultProps() {
    return {
      title: 'Undefined Product',
      price: 100,
      initialQty: 0
    }
}


static propTypes = {
  title: React.PropTypes.string.isRequired,
  price: React.PropTypes.number.isRequired,
  initialQty: React.PropTypes.number
}
```

替换成下面的代码：

```js
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
```

## ES7 Property Initialiazers for initial state of React component

最后一步将初始状态从构造函数中转变成属性初始化。

在`CartItem`构造函数的后天添加正确的代码：
```js
export default class CartItem extends React.Component {

    // .. constructor starts here
    state = {
        qty: this.props.initialQty,
        total: 0
    };
    // .. some code here
```
你需要把状态初始化代码从构造函数中删除。

最后你需要检查一下`cartItem.jsx`文件里面完整的代码：

```js
import React from 'react';

export default class CartItem extends React.Component {
    constructor(props) {
        super(props);
    }

    state = {
        qty: this.props.initialQty,
        total: 0
    };

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
        )
    }
}
```


## 结论
在这一节中，我们熟练掌握了ES6和ES7 React组建属性的初始化，类型绑定。

下一小结，我们继续研究React+ES6系列教程。

## 参考文档
- [Props vs state in React](https://github.com/uberVU/react-guide/blob/master/props-vs-state.md)
- [About Prop Validation and more, official React documentation](https://facebook.github.io/react/docs/components-and-props.html)
- [About experimental features in BabelJS](https://babeljs.io/docs/usage/experimental/)
- [ES7 Class Properties Proposal](https://gist.github.com/jeffmo/054df782c05639da2adb)
