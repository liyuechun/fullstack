### 箭头函数

一个箭头函数表达式的语法比一个函数表达式更短，并且不绑定自己的 this，arguments，super或 new.target。

```JavaScript
// Expression bodies
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);
var pairs = evens.map(v => ({
    even: v,
    odd: v + 1
}));

// Statement bodies
nums.forEach(v => {
    if (v % 5 === 0) 
        fives.push(v);
    }
);

// Lexical this
var bob = {
    _name: "Bob",
    _friends: [],
    printFriends() {
        this
            ._friends
            .forEach(f => console.log(this._name + " knows " + f));
    }
}
```

更多信息: [MDN Arrow Functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

