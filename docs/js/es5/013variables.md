# 变量

## 总是使用`const`来声明变量。 不这样做会导致全局变量。 我们想避免污染全局命名空间。 

```javascript
// bad
superPower = new SuperPower();

// good
const superPower = new SuperPower();
```

## 使用一个 `const` 声明一个变量

eslint rules: [`one-var`](http://eslint.org/docs/rules/one-var.html).

```javascript
// bad
const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';

// bad
// (compare to above, and try to spot the mistake)
const items = getItems(),
    goSportsTeam = true;
dragonball = 'z';

// good
const items = getItems();
const goSportsTeam = true;
const dragonball = 'z';
```

## Group all your `const`s and then group all your `let`s.

> Why? This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

```javascript
// bad
let i, len, dragonball,
    items = getItems(),
    goSportsTeam = true;

// bad
let i;
const items = getItems();
let dragonball;
const goSportsTeam = true;
let len;

// good
const goSportsTeam = true;
const items = getItems();
let dragonball;
let i;
let length;
```

## Assign variables where you need them, but place them in a reasonable place.

> Why? `let` and `const` are block scoped and not function scoped.

```javascript
// good
function () {
    test();
    console.log('doing stuff..');

    //..other stuff..

    const name = getName();

    if (name === 'test') {
        return false;
    }

    return name;
}

// bad ## unnecessary function call
function (hasName) {
    const name = getName();

    if (!hasName) {
        return false;
    }

    this.setFirstName(name);

    return true;
}

// good
function (hasName) {
    if (!hasName) {
        return false;
    }

    const name = getName();
    this.setFirstName(name);

    return true;
}
```



