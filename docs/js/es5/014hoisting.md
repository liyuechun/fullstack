# 变量提升(Hoisting)

## var、const、let

`var`声明被提升到其范围的顶部，它们的赋值没有。`const`和`let`的声明有一个叫做 [Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let)的新概念。知道[typeof is no longer safe](http://es-discourse.com/t/why-typeof-is-no-longer##safe/15)是非常重要的。

```javascript
// we know this wouldn't work (assuming there
// is no notDefined global variable)
function example() {
    console.log(notDefined); // => throws a ReferenceError
}

// creating a variable declaration after you
// reference the variable will work due to
// variable hoisting. Note: the assignment
// value of `true` is not hoisted.
function example() {
    console.log(declaredButNotAssigned); // => undefined
    var declaredButNotAssigned = true;
}

// The interpreter is hoisting the variable
// declaration to the top of the scope,
// which means our example could be rewritten as:
function example() {
    let declaredButNotAssigned;
    console.log(declaredButNotAssigned); // => undefined
    declaredButNotAssigned = true;
}

// using const and let
function example() {
    console.log(declaredButNotAssigned); // => throws a ReferenceError
    console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
    const declaredButNotAssigned = true;
}
```

## 匿名函数表达式提升其变量名，而不是函数赋值

```javascript
function example() {
    console.log(anonymous); // => undefined

    anonymous(); // => TypeError anonymous is not a function

    var anonymous = function () {
        console.log('anonymous function expression');
    };
}
```

## 命名函数表达式提升变量名称，而不是函数名称或函数体。

```javascript
function example() {
    console.log(named); // => undefined

    named(); // => TypeError named is not a function

    superPower(); // => ReferenceError superPower is not defined

    var named = function superPower() {
        console.log('Flying');
    };
}

// the same is true when the function name
// is the same as the variable name.
function example() {
    console.log(named); // => undefined

    named(); // => TypeError named is not a function

    var named = function named() {
        console.log('named');
    }
}
```

## 函数声明提升其名称和函数体。

```javascript
function example() {
    superPower(); // => Flying

    function superPower() {
        console.log('Flying');
    }
}
```

## 关于更多 [JavaScript 作用域 & 变量提升](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/) by [Ben Cherry](http://www.adequatelygood.com/).

