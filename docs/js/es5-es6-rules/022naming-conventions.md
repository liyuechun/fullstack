# 命名约定

## 避免单字母名称，通过词语描述你的命名

```javascript
// bad
function q() {
    // ...stuff...
}

// good
function query() {
    // ..stuff..
}
```

## 当命名对象、函数和实例是使用驼峰的写法

eslint rules: [`camelcase`](http://eslint.org/docs/rules/camelcase.html).

```javascript
// bad
const OBJEcttsssss = {};
const this_is_my_object = {};
function c() {}

// good
const thisIsMyObject = {};
function thisIsMyFunction() {}
```

## 命名类时让单词的每个首字母都大写

```javascript
// bad
function user(options) {
    this.name = options.name;
}

const bad = new user({name: 'nope'});

// good
class User {
    constructor(options) {
        this.name = options.name;
    }
}

const good = new User({name: 'yup'});
```

## 当命名私有属性时，使用`_`开头
eslint rules: [`no-underscore-dangle`](http://eslint.org/docs/rules/no-underscore-dangle.html).

```javascript
// bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';

// good
this._firstName = 'Panda';
```

## 函数中需要使用 `this` 时，请使用箭头函数或者函数绑定

```javascript
// bad
function foo() {
    const self = this;
    return function () {
        console.log(self);
    };
}

// bad
function foo() {
    const that = this;
    return function () {
        console.log(that);
    };
}

// good
function foo() {
    return () => {
        console.log(this);
    };
}
```

## 如果你的文件只导出一个单一的类，那么导入时的名字和导出的名字一致

```javascript
// file contents
class CheckBox {
    // ...
}
export default CheckBox;

// in some other file bad
import CheckBox from './checkBox';

// bad
import CheckBox from './check_box';

// good
import CheckBox from './CheckBox';
```

## 导出函数时，函数名使用驼峰的写法

```javascript
function makeStyleGuide() {}

export default makeStyleGuide;
```

## 导出单例/函数库/裸对象时使用全单词首字母大写

```javascript
const AirbnbStyleGuide = {
    es6: {}
};

export default AirbnbStyleGuide;
```


