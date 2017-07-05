### Template Strings
Template strings provide syntactic sugar for constructing strings.  This is similar to string interpolation features in Perl, Python and more.  Optionally, a tag can be added to allow the string construction to be customized, avoiding injection attacks or constructing higher level data structures from string contents.

```JavaScript
// Basic literal string creation
`In JavaScript '\n' is a line-feed.`

// Multiline strings
`In JavaScript this is
 not legal.`

// String interpolation
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

// Construct an HTTP request prefix is used to interpret the replacements and construction
POST`http://foo.org/bar?a=${a}&b=${b}
     Content-Type: application/json
     X-Credentials: ${credentials}
     { "foo": ${foo},
       "bar": ${bar}}`(myOnReadyStateChangeHandler);
```

- Use single quotes `''` for strings.

  eslint rules: [`quotes`](http://eslint.org/docs/rules/quotes.html).

  ```javascript
  // bad
  const name = "Capt. Janeway";

  // good
  const name = 'Capt. Janeway';
  ```

- Strings that cause the line to go over 100 characters should be written across multiple lines using string concatenation.
- Note: If overused, long strings with concatenation could impact performance. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

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

- When programmatically building up strings, use template strings instead of concatenation.

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
- Never use `eval()` on a string, it opens too many vulnerabilities.


More info: [MDN Template Strings](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/template_strings)