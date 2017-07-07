# 访问器

## `getVal()` 和 `setVal('hello')`

```javascript
// bad
dragon.age();

// good
dragon.getAge();

// bad
dragon.age(25);

// good
dragon.setAge(25);
```

## 如果属性类型为 `boolean`, 使用 `isVal()` 或者 `hasVal()`

```javascript
// bad
if (!dragon.age()) {
    return false;
}

// good
if (!dragon.hasAge()) {
    return false;
}

```

## 可以创建`get()`和`set()`函数，但要保持一致

```javascript
class Jedi {
    constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
    }

    set(key, val) {
        this[key] = val;
    }

    get(key) {
        return this[key];
    }
}
```

