# 构造函数

## 总是使用`class`，避免直接操作`prototype`

> 为什么? `class` 语法更简洁并且可读性更强。

```javascript
// bad
function Queue(contents = []) {
    this._queue = [...contents];
}
Queue.prototype.pop = function () {
    const value = this._queue[0];
    this._queue.splice(0, 1);
    return value;
}


// good
class Queue {
    constructor(contents = []) {
        this._queue = [...contents];
    }
    pop() {
        const value = this._queue[0];
        this._queue.splice(0, 1);
        return value;
    }
}
```

## 使用 `extends` 来实现继承

> 为什么? 它是一种内置的方式来继承原型功能而不会破坏`instanceof`。

```javascript
// bad
const inherits = require('inherits');

function PeekableQueue(contents) {
    Queue.apply(this, contents);
}
inherits(PeekableQueue, Queue);
PeekableQueue.prototype.peek = function () {
    return this._queue[0];
}

// good
class PeekableQueue extends Queue {
    peek() {
        return this._queue[0];
    }
}
```

## 方法可以返回`this`来帮助方法链接。

```javascript
// bad
Jedi.prototype.jump = function () {
    this.jumping = true;
    return true;
};

Jedi.prototype.setHeight = function (height) {
    this.height = height;
};

const luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20); // => undefined

// good
class Jedi {
    jump() {
        this.jumping = true;
        return this;
    }

    setHeight(height) {
        this.height = height;
        return this;
    }
}

const luke = new Jedi();

luke.jump()
    .setHeight(20);
```


