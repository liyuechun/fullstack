# 箭头函数

## 当您必须使用函数表达式（如传递匿名函数时），请使用箭头函数表示法。

> 为什么? 它创建一个在`this`的上下文中执行的函数的版本，这通常是你想要的，并且是一个更简洁的语法。


> 为什么不呢? 如果你有一个相当复杂的功能，你可以将这个逻辑移到自己的函数声明中。

eslint rules: [`prefer-arrow-callback`](http://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](http://eslint.org/docs/rules/arrow-spacing.html).

```javascript
// bad
[1, 2, 3].map(function (x) {
    const y = x + 1;
    return x * y;
});

// good
[1, 2, 3].map((x) => {
    const y = x + 1;
    return x * y;
});
```

## 如果函数体由单个表达式组成，则省略大括号并使用隐式返回。 否则，保留大括号并使用`return`语句。


> 为什么? 语法糖，当多个功能链接在一起时，它可读性很好。

> 为什么不呢? 如果您计划返回一个对象。

eslint rules: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](http://eslint.org/docs/rules/arrow-body-style.html).

```javascript
// good
[1, 2, 3].map(number => `A string containing the ${number}.`);

// bad
[1, 2, 3].map(number => {
    const nextNumber = number + 1;
    `A string containing the ${nextNumber}.`;
});

// good
[1, 2, 3].map(number => {
    const nextNumber = number + 1;
    return `A string containing the ${nextNumber}.`;
});
```

## 如果表达式跨越多行，请将其包装在括号中以获得更好的可读性。

> 为什么? 它清楚地显示了函数的开始和结束。


```js
// bad
[1, 2, 3].map(number => 'As time went by, the string containing the ' +
    `${number} became much longer. So we needed to break it over multiple ` +
    'lines.'
);

// good
[1, 2, 3].map(number => (
    `As time went by, the string containing the ${number} became much ` +
    'longer. So we needed to break it over multiple lines.'
));
```


## If your function takes a single argument and doesn’t use braces, omit the parentheses. Otherwise, always include parentheses around arguments.
如果你的函数只有一个参数，能不使用大括号，则省略大括号。

> 为什么? 少视觉混乱。

eslint rules: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html).

```js
// bad
[1, 2, 3].map((x) => x * x);

// good
[1, 2, 3].map(x => x * x);

// good
[1, 2, 3].map(number => (
    `A long string with the ${number}. It’s so long that we’ve broken it ` +
    'over multiple lines!'
));

// bad
[1, 2, 3].map(x => {
    const y = x + 1;
    return x * y;
});

// good
[1, 2, 3].map((x) => {
    const y = x + 1;
    return x * y;
});
```

