# 类型转换 & 强制类型转换

## 在语句的开始执行类型强制转换

## Strings:

```javascript
//  => this.reviewScore = 9;

// bad
const totalScore = this.reviewScore + '';

// good
const totalScore = String(this.reviewScore);
```

## Numbers: 使用`Number`进行类型转换，`parseInt`总是用一个基数来解析字符串。

eslint rules: [`radix`](http://eslint.org/docs/rules/radix).

```javascript
const inputValue = '4';

// bad
const val = new Number(inputValue);

// bad
const val = +inputValue;

// bad
const val = inputValue >> 0;

// bad
const val = parseInt(inputValue);

// good
const val = Number(inputValue);

// good
const val = parseInt(inputValue, 10);
```

## 位移(Bitshift)

```javascript
// good
/**
* parseInt was the reason my code was slow.
* Bitshifting the String to coerce it to a
* Number made it a lot faster.
*/
const val = inputValue >> 1;
```

## Booleans:

```javascript
const age = 0;

// bad
const hasAge = new Boolean(age);

// good
const hasAge = Boolean(age);

// good
const hasAge = !!age;
```

