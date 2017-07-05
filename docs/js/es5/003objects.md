## 对象

1. 使用字面量创建对象

eslint rules: [`no-new-object`](http://eslint.org/docs/rules/no-new-object.html).

```javascript
// bad
const item = new Object();

// good
const item = {};
```

- 不要使用保留字

```javascript
// bad
const superman = {
    default: {
        clark: 'kent'
    },
    private: true
};

// good
const superman = {
    defaults: {
        clark: 'kent'
    },
    hidden: true
};
```

1. 使用可读的同义词代替保留字。

```javascript
// bad
const superman = {
    class: 'alien'
};

// bad
const superman = {
    klass: 'alien'
};

// good
const superman = {
    type: 'alien'
};
```

1. 创建具有动态属性名称的对象时，请使用可计算的属性命名。

> 为什么? 它们允许您在一个位置定义对象的所有属性。

```javascript
function getKey(k) {
    return `a key named ${k}`;
}

// bad
const obj = {
    id: 5,
    name: 'San Francisco'
};
obj[getKey('enabled')] = true;

// good
const obj = {
    id: 5,
    name: 'San Francisco',
    [getKey('enabled')]: true
};  
```

1. 使用对象方法简写。

eslint rules: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html).

```javascript
// bad
const atom = {
    value: 1,

    addValue: function (value) {
        return atom.value + value;
    }
};

// good
const atom = {
    value: 1,

    addValue(value) {
        return atom.value + value;
    }
};
```

1. 使用属性值简写

> 为什么? 简洁，清晰。

  eslint rules: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html).


```javascript
const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
    lukeSkywalker: lukeSkywalker
};

// good
const obj = {
    lukeSkywalker
};
```

1. 对象声明中属性使用简写之前，先做值声明。

> 为什么? 它更容易知道哪些属性使用了简写。


```js
const anakinSkywalker = 'Anakin Skywalker';
const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
    episodeOne: 1,
    twoJediWalkIntoACantina: 2,
    lukeSkywalker,
    episodeThree: 3,
    mayTheFourth: 4,
    anakinSkywalker
};

// good
const obj = {
    lukeSkywalker,
    anakinSkywalker,
    episodeOne: 1,
    twoJediWalkIntoACantina: 2,
    episodeThree: 3,
    mayTheFourth: 4
};
```

1. 只给无效标识符的属性添加引号。

> 为什么? 一般来说，我们认为它主观上更容易阅读。 它改进了语法高亮，并且更容易被许多JS引擎优化。

eslint rules: [`quote-props`](http://eslint.org/docs/rules/quote-props.html).

```javascript
// bad
const bad = {
    'foo': 3,
    'bar': 4,
    'data-blah': 5
};

// good
const good = {
    foo: 3,
    bar: 4,
    'data-blah': 5
};
```

