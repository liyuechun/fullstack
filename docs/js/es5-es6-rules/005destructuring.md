# 解构

## 访问和使用对象的多个属性时，使用对象解构。

  > 为什么? 结构可以帮助您避免为这些属性创建临时引用。


```javascript
// bad
function getFullName(user) {
    const firstName = user.firstName;
    const lastName = user.lastName;

    return `${firstName} ${lastName}`;
}

// good
function getFullName(user) {
    const {
        firstName,
        lastName
    } = user;
    return `${firstName} ${lastName}`;
}

// best
function getFullName({firstName,lastName}) {
    return `${firstName} ${lastName}`;
}
```

## 使用数组解构

```javascript
const arr = [1, 2, 3, 4];

// bad
const first = arr[0];
const second = arr[1];

// good
const [first, second] = arr;
```

## 对多个返回值使用对象解析，而不是数组解构。

> 为什么? 你可以增加新的属性或改变事物的秩序而不破坏调用点。

```javascript
// bad
function processInput(input) {
    // then a miracle occurs
    return [left, right, top, bottom];
}

// the caller needs to think about the order of return data
const [left, __, top] = processInput(input);

// good
function processInput(input) {
    // then a miracle occurs
    return {
        left,
        right,
        top,
        bottom
    };
}

// the caller selects only the data they need
const {
    left,
    right
} = processInput(input);
```


