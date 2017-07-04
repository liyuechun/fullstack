### Strings

  1. Use single quotes `''` for strings.

    eslint rules: [`quotes`](http://eslint.org/docs/rules/quotes.html).

    ```javascript
    // bad
    const name = "Capt. Janeway";

    // good
    const name = 'Capt. Janeway';
    ```

  1. Strings that cause the line to go over 100 characters should be written across multiple lines using string concatenation.

  1. Note: If overused, long strings with concatenation could impact performance. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

    ```javascript
    // bad
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // good
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  1. When programmatically building up strings, use template strings instead of concatenation.

    > Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features.

    eslint rules: [`prefer-template`](http://eslint.org/docs/rules/prefer-template.html).

    ```javascript
    // bad
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // bad
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // good
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```
  1. Never use `eval()` on a string, it opens too many vulnerabilities.