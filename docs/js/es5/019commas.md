### Commas

- Leading commas: **Nope.**

  eslint rules: [`comma-style`](http://eslint.org/docs/rules/comma-style.html).

  ```javascript
  // bad
  const story = [
      once
    , upon
    , aTime
  ];

  // good
  const story = [
    once,
    upon,
    aTime,
  ];

  // bad
  const hero = {
      firstName: 'Ada'
    , lastName: 'Lovelace'
    , birthYear: 1815
    , superPower: 'computers'
  };

  // good
  const hero = {
    firstName: 'Ada',
    lastName: 'Lovelace',
    birthYear: 1815,
    superPower: 'computers',
  };
  ```

- Additional trailing comma: **Yup.**

  eslint rules: [`comma-dangle`](http://eslint.org/docs/rules/comma-dangle.html).

  > Why? This leads to cleaner git diffs. Also, transpilers like Babel will remove the additional trailing comma in the transpiled code which means you don't have to worry about the [trailing comma problem](es5/README.md#commas) in legacy browsers.

  ```javascript
  // bad - git diff without trailing comma
  const hero = {
        firstName: 'Florence',
  -    lastName: 'Nightingale'
  +    lastName: 'Nightingale',
  +    inventorOf: ['coxcomb graph', 'modern nursing']
  };

  // good - git diff with trailing comma
  const hero = {
        firstName: 'Florence',
        lastName: 'Nightingale',
  +    inventorOf: ['coxcomb chart', 'modern nursing'],
  };

  // bad
  const hero = {
    firstName: 'Dana',
    lastName: 'Scully'
  };

  const heroes = [
    'Batman',
    'Superman'
  ];

  // good
  const hero = {
    firstName: 'Dana',
    lastName: 'Scully',
  };

  const heroes = [
    'Batman',
    'Superman',
  ];
  ```