# 函数

## 使用函数声明代替函数表达式

> 为什么? 函数声明被命名，所以它们在调用堆栈中更容易识别。而且，它是一整个函数声明的整体被悬挂，而不只是一个函数表达式的引用被悬挂。

```javascript
// bad
const foo = function () {};

// good
function foo() {}
```

## 函数表达式：

```javascript
// immediately##invoked function expression (IIFE)
(() => {
    console.log('Welcome to the Internet. Please follow me.');
})();
```

## 绝不在一个非函数块(if,while,等等)中声明一个函数。
绝不在一个非函数块(if,while,等等)中声明一个函数。而是将一个函数赋值给一个变量。其实浏览器允许你在非函数块中声明函数，但有一个坏消息，它们会以不同的方式来解释。

```javascript
// bad
if (currentUser) {
    function test() {
        console.log('Nope.');
    }
}

// good
let test;
if (currentUser) {
    test = () => {
        console.log('Yup.');
    };
}
```

## 绝不命名名字为`arguments`的参数。

绝不命名名字为`arguments`的参数。，这将优先于给予每个函数范围的`arguments`对象。

```javascript
// bad
function nope(name, options, arguments) {
    // ...stuff...
}

// good
function yup(name, options, args) {
    // ...stuff...
}
```

## 不要使用`arguments`，而是选择使用`rest`语法`...`

  ```javascript
  // bad
  function concatenateAll() {
    const args = Array.prototype.slice.call(arguments);
    return args.join('');
  }

  // good
  function concatenateAll(...args) {
    return args.join('');
  }
  ```

## 使用默认参数语法而不是变异函数参数

```javascript
// really bad
function handleThings(opts) {
    // No! We shouldn't mutate function arguments. Double bad: if opts is falsy
    // it'll be set to an object which may be what you want but it can introduce
    // subtle bugs.
    opts = opts || {};
    // ...
}

// still bad
function handleThings(opts) {
    if (opts === void 0) {
        opts = {};
    }
    // ...
}

// good
function handleThings(opts = {}) {
    // ...
}
```

## 使用默认参数避免副作用。

> 为什么? 他们有令人困惑的理由。

```javascript
var b = 1;
// bad
function count(a = b++) {
    console.log(a);
}
count(); // 1
count(); // 2
count(3); // 3
count(); // 3
```

## 最后一个参数总放默认值

```javascript
// bad
function handleThings(opts = {}, name) {
    // ...
}

// good
function handleThings(name, opts = {}) {
    // ...
}
```

## 绝不实用构造函数创建一个新函数

```javascript
// bad
var add = new Function('a', 'b', 'return a + b');

// still bad
var subtract = Function('a', 'b', 'return a - b');
```

## 间隔函数进行签名

> 为什么? 一致性很好，您不必在添加或删除名称时添加或删除空格。


```javascript
// bad
const f = function(){};
const g = function (){};
const h = function() {};

// good
const x = function () {};
const y = function a() {};
```


## 不要突变参数。

> 为什么? 操作作为参数传入的对象可能会导致原始调用者中不必要的可变副作用。

eslint rules: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html).

```javascript
// bad
function f1(obj) {
    obj.key = 1;
};

// good
function f2(obj) {
    const key = Object
        .prototype
        .hasOwnProperty
        .call(obj, 'key')
        ? obj.key
        : 1;
};
```

## 绝不给参数重新赋值

> 为什么? 重新分配参数可能导致意外的行为，特别是访问`arguments`对象时。 它也可能导致优化问题，特别是在V8中。

eslint rules: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html).

```javascript
// bad
function f1(a) {
    a = 1;
}

function f2(a) {
    if (!a) {
        a = 1;
    }
}

// good
function f3(a) {
    const b = a || 1;
}

function f4(a = 1) {}
```

