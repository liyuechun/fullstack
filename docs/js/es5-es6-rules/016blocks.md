# 块（Blocks）

## 多行时请使用括号括起来

```javascript
// bad
if (test)
    return false;

// good
if (test) return false;

// good
if (test) {
    return false;
}

// bad
function () {return false;}

// good
function () {
    return false;
}
```

## `if` 和 `else`

如果使用`if` 和 `else`时，`if` 和 `else`都是用blocks，并且将`else`放在`if`语句结束`}`的同一行。

eslint rules: [`brace-style`](http://eslint.org/docs/rules/brace-style.html).

```javascript
// bad
if (test) {
    thing1();
    thing2();
} 
else {
    thing3();
}

// good
if (test) {
    thing1();
    thing2();
} else {
    thing3();
}
```

