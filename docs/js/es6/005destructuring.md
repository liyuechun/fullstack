### 解构
解构赋值 语法是一个Javascript表达式，这使得可以将值从数组或属性从对象提取到不同的变量中。

```JavaScript
// list matching
var [a,,b] = [1, 2, 3];

// object matching
var {
    op: a,
    lhs: {
        op: b
    },
    rhs: c
} = getASTNode()

// object matching shorthand binds `op`, `lhs` and `rhs` in scope
var {op, lhs, rhs} = getASTNode()

// Can be used in parameter position
function g({name: x}) {
    console.log(x);
}

g({name: 5})

// Fail-soft destructuring
var [a] = [];
a === undefined;

// Fail-soft destructuring with defaults
var [a = 1] = [];
a === 1;
```

更多信息: [MDN Destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

