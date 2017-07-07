# 属性

## 使用点语法访问属性

eslint rules: [`dot-notation`](http://eslint.org/docs/rules/dot-notation.html).

```javascript
const luke = {
    jedi: true,
    age: 28,
};

// bad
const isJedi = luke['jedi'];

// good
const isJedi = luke.jedi;
```

## 使用下标符号`[]`访问具有变量的属性

```javascript
const luke = {
    jedi: true,
    age: 28,
};

function getProp(prop) {
    return luke[prop];
}

const isJedi = getProp('jedi');
```


