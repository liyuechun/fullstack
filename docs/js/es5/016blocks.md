### Blocks

  1. Use braces with all multi-line blocks.

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function () { return false; }

    // good
    function () {
      return false;
    }
    ```

  1. If you're using multi-line blocks with `if` and `else`, put `else` on the same line as your
    `if` block's closing brace.

    eslint rules: [`brace-style`](http://eslint.org/docs/rules/brace-style.html).

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```