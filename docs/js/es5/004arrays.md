## Arrays

  - [1.1](#1.1) <a name='1.1'></a> Use the literal syntax for array creation.

    eslint rules: [`no-array-constructor`](http://eslint.org/docs/rules/no-array-constructor.html).

    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  - [1.2](#1.2) <a name='1.2'></a> Use Array#push instead of direct assignment to add items to an array.

    ```javascript
    const someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a>
  - [1.3](#1.3) <a name='1.3'></a> Use array spreads `...` to copy arrays.

    ```javascript
    // bad
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    const itemsCopy = [...items];
    ```
  - [1.4](#1.4) <a name='1.4'></a> To convert an array-like object to an array, use Array#from.

    ```javascript
    const foo = document.querySelectorAll('.foo');
    const nodes = Array.from(foo);
    ```
