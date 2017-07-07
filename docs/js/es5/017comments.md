# 注释（Comments）

## 多行注释

使用 `/** ... */` 来进行多行注释。包括描述，指定所有参数和返回值的类型和值。

```javascript
// bad
// make() returns a new element
// based on the passed in tag name
//
// @param {String} tag
// @return {Element} element
function make(tag) {

    // ...stuff...

    return element;
}

// good
/**
 * make() returns a new element
 * based on the passed in tag name
 *
 * @param {String} tag
 * @return {Element} element
 */
function make(tag) {

    // ...stuff...

    return element;
}
```

## 单行注释
使用 `//` 进行单行注释。 在要注释的代码的上面添加一行注释。每一个单行注释的上面请流出一个空行。

```javascript
// bad
const active = true; // is current tab

// good
// is current tab
const active = true;

// bad
function getType() {
    console.log('fetching type...');
    // set the default type to 'no type'
    const type = this._type || 'no type';

    return type;
}

// good
function getType() {
    console.log('fetching type...');

    // set the default type to 'no type'
    const type = this._type || 'no type';

    return type;
}

// also good
function getType() {
    // set the default type to 'no type'
    const type = this._type || 'no type';

    return type;
}
```

## `FIXME` 和 `TODO`

- 使用 `// FIXME:` 注释问题

```javascript
class Calculator extends Abacus {
    constructor() {
        super();

        // FIXME: shouldn't use a global here
        total = 0;
    }
}
```

- 使用 `// TODO:` 注释问题的解决方案。

```javascript
class Calculator extends Abacus {
    constructor() {
        super();

        // TODO: total should be configurable by an options param
        this.total = 0;
    }
}
```

