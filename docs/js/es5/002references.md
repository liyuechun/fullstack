### References

   1. Use `const` for all of your references; avoid using `var`.

    > Why? This ensures that you can't reassign your references, which can lead to bugs and difficult to comprehend code.

    eslint rules: [`prefer-const`](http://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](http://eslint.org/docs/rules/no-const-assign.html).

    ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```
  1. If you must reassign references, use `let` instead of `var`.

    > Why? `let` is block-scoped rather than function-scoped like `var`.

    eslint rules: [`no-var`](http://eslint.org/docs/rules/no-var.html).

    ```javascript
    // bad
    var count = 1;
    if (true) {
      count += 1;
    }

    // good, use the let.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```
  1. Note that both `let` and `const` are block-scoped.

    ```javascript
    // const and let only exist in the blocks they are defined in.
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```
