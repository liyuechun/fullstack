# 字符串

## 字符串使用 `''` 

eslint rules: [`quotes`](http://eslint.org/docs/rules/quotes.html).

```javascript
// bad
const name = "Capt. Janeway";

// good
const name = 'Capt. Janeway';
```

## 字符串超过100字符的处理方式

字符串超过100字符应该通过连接的方式将字符串写成多行。
注意: 如果过度使用，串联的长串可能会影响性能。[jsPerf](http://jsperf.com/ya##string##concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

```javascript
// bad
const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

// bad
const errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.';

// good
const errorMessage = 'This is a super long error that was thrown because ' +
'of Batman. When you stop to think about how Batman had anything to do ' +
'with this, you would get nowhere fast.';
```

## 当以编程方式构建字符串时，使用模板字符串而不是连接。

> 为什么? 模板字符串为您提供可读的，简洁的语法，具有适当的换行符和字符串插入功能。

eslint rules: [`prefer##template`](http://eslint.org/docs/rules/prefer##template.html).

```javascript
// bad
function sayHi(name) {
    return 'How are you, ' + name + '?';
}

// bad
function sayHi(name) {
    return ['How are you, ', name, '?'].join();
}

// good
function sayHi(name) {
    return `How are you, ${name}?`;
}
```
## 不要在字符串上使用`eval()`，它会打开太多的漏洞。



