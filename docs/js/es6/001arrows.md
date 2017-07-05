### Arrows
Arrows are a function shorthand using the `=>` syntax.  They are syntactically similar to the related feature in C#, Java 8 and CoffeeScript.  They support both statement block bodies as well as expression bodies which return the value of the expression.  Unlike functions, arrows share the same lexical `this` as their surrounding code.

```JavaScript
// Expression bodies
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);
var pairs = evens.map(v => ({even: v, odd: v + 1}));

// Statement bodies
nums.forEach(v => {
  if (v % 5 === 0)
    fives.push(v);
});

// Lexical this
var bob = {
  _name: "Bob",
  _friends: [],
  printFriends() {
    this._friends.forEach(f =>
      console.log(this._name + " knows " + f));
  }
}
```

- When you must use function expressions (as when passing an anonymous function), use arrow function notation.

> Why? It creates a version of the function that executes in the context of `this`, which is usually what you want, and is a more concise syntax.

> Why not? If you have a fairly complicated function, you might move that logic out into its own function declaration.

eslint rules: [`prefer-arrow-callback`](http://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](http://eslint.org/docs/rules/arrow-spacing.html).

```javascript
// bad
[1, 2, 3].map(function (x) {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```

- If the function body consists of a single expression, omit the braces and use the implicit return. Otherwise, keep the braces and use a `return` statement.

> Why? Syntactic sugar. It reads well when multiple functions are chained together.

> Why not? If you plan on returning an object.

eslint rules: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](http://eslint.org/docs/rules/arrow-body-style.html).

```javascript
// good
[1, 2, 3].map(number => `A string containing the ${number}.`);

// bad
[1, 2, 3].map(number => {
  const nextNumber = number + 1;
  `A string containing the ${nextNumber}.`;
});

// good
[1, 2, 3].map(number => {
  const nextNumber = number + 1;
  return `A string containing the ${nextNumber}.`;
});
```

- In case the expression spans over multiple lines, wrap it in parentheses for better readability.

> Why? It shows clearly where the function starts and ends.

```js
// bad
[1, 2, 3].map(number => 'As time went by, the string containing the ' +
  `${number} became much longer. So we needed to break it over multiple ` +
  'lines.'
);

// good
[1, 2, 3].map(number => (
  `As time went by, the string containing the ${number} became much ` +
  'longer. So we needed to break it over multiple lines.'
));
```


- If your function takes a single argument and doesn’t use braces, omit the parentheses. Otherwise, always include parentheses around arguments.

> Why? Less visual clutter.

eslint rules: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html).

```js
// bad
[1, 2, 3].map((x) => x * x);

// good
[1, 2, 3].map(x => x * x);

// good
[1, 2, 3].map(number => (
  `A long string with the ${number}. It’s so long that we’ve broken it ` +
  'over multiple lines!'
));

// bad
[1, 2, 3].map(x => {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```




More info: [MDN Arrow Functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)