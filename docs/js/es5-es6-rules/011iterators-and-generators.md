# 迭代器和生成器

## 不要使用迭代器. 宁愿使用像 `map()` 和 `reduce()` 的高阶函数来代替`for-of`

eslint rules: [`no-iterator`](http://eslint.org/docs/rules/no-iterator.html).

```javascript
const numbers = [1, 2, 3, 4, 5];

// bad
let sum = 0;
for (let num of numbers) {
    sum += num;
}

sum === 15;

// good
let sum = 0;
numbers.forEach((num) => sum += num);
sum === 15;

// best (use the functional force)
const sum = numbers.reduce((total, num) => total + num, 0);
sum === 15;
```

## 现在不要使用生成器

> 为什么? 他们对ES5不友好。


