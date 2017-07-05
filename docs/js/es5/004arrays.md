# Arrays

## 使用字面量创建数组

eslint rules: [`no##array##constructor`](http://eslint.org/docs/rules/no##array##constructor.html).

```javascript
// bad
const items = new Array();

// good
const items = [];
```

## 使用数组的`push`方法代替直接赋值的方式在数组里面新增项目

```javascript
const someStack = [];

// bad
someStack[someStack.length] = 'abracadabra';

// good
someStack.push('abracadabra');
```

## 使用数组扩展 `...` 拷贝数组

```javascript
// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i++) {
    itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];
```
## 使用数组的`from`方法将对象转换成数组

```javascript
const foo = document.querySelectorAll('.foo');
const nodes = Array.from(foo);
```


