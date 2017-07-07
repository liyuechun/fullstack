# 比较运算与恒等

## 使用 `===` 和 `!==` 而不是 `==` 和 `!=`.

## 条件语句

条件语句如`if`语句使用`ToBoolean`抽象方法强制来评估它们的表达式，并且始终遵循这些简单的规则：

eslint rules: [`eqeqeq`](http://eslint.org/docs/rules/eqeqeq.html).

+ **Objects** 评估为 **true**
+ **Undefined** 评估为 **false**
+ **Null** 评估为 **false**
+ **Booleans** 评估为 **the value of the boolean**
+ **Numbers** 如果 **+0, -0, 或者 NaN** 评估为 **false** , 否则为 **true**
+ **Strings** `''`空字符串评估为 **false** , 否则为 **true**

```javascript
if ([0]) {
    // true
    // An array is an object, objects evaluate to true
}
```

## 使用短句

```javascript
// bad
if (name !== '') {
    // ...stuff...
}

// good
if (name) {
    // ...stuff...
}

// bad
if (collection.length > 0) {
    // ...stuff...
}

// good
if (collection.length) {
    // ...stuff...
}
```

## 关于更多，请移步 [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth##equality##and##javascript/#more##2108) by Angus Croll.

