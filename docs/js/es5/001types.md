## 类型

1. **原始类型**: 当您访问原始类型时，直接对其值进行工作。

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

```javascript
const foo = 1;
let bar = foo;

bar = 9;

console.log(foo, bar); // => 1, 9
```

2. **复杂类型**: 当您访问复杂类型时，您可以直接操作它的值引用。

    + `object`
    + `array`
    + `function`

```javascript
const foo = [1, 2];
const bar = foo;

bar[0] = 9;

console.log(foo[0], bar[0]); // => 9, 9
```


