### 进行函数传递

举例来说，你可以这样做：

```js
Last login: Thu Jun 29 20:03:25 on ttys001
liyuechun:~ yuechunli$ node
> function say(word) {
...   console.log(word);
... }
undefined
> 
> function execute(someFunction, value) {
...   someFunction(value);
... }
undefined
> 
> execute(say, "Hello");
Hello
undefined
> 
```

请仔细阅读这段代码！在这里，我们把 say 函数作为execute函数的第一个变量进行了传递。这里传递的不是 say 的返回值，而是 say 本身！

这样一来， say 就变成了execute 中的本地变量 someFunction ，execute可以通过调用 someFunction() （带括号的形式）来使用 say 函数。

当然，因为 say 有一个变量， execute 在调用 someFunction 时可以传递这样一个变量。

我们可以，就像刚才那样，用它的名字把一个函数作为变量传递。但是我们不一定要绕这个“先定义，再传递”的圈子，我们可以直接在另一个函数的括号中定义和传递这个函数：

```js
Last login: Thu Jun 29 20:04:35 on ttys001
liyuechun:~ yuechunli$ node
> function execute(someFunction, value) {
...   someFunction(value);
... }
undefined
> 
> execute(function(word){ console.log(word) }, "Hello");
Hello
undefined
> 
```

我们在 execute 接受第一个参数的地方直接定义了我们准备传递给 execute 的函数。

用这种方式，我们甚至不用给这个函数起名字，这也是为什么它被叫做 匿名函数 。

这是我们和我所认为的“进阶”JavaScript的第一次亲密接触，不过我们还是得循序渐进。现在，我们先接受这一点：在JavaScript中，一个函数可以作为另一个函数接收一个参数。我们可以先定义一个函数，然后传递，也可以在传递参数的地方直接定义函数。