# 模块

> 文章出处：从零到壹全栈部落

## 始终在非标准模块系统上使用模块（`import` /`export`）。 您可以随时浏览您的首选模块系统。

> 为什么? 模块是未来，让我们开始使用未来。

```javascript
// bad
const AirbnbStyleGuide = require('./AirbnbStyleGuide');
module.exports = AirbnbStyleGuide.es6;

// ok
import AirbnbStyleGuide from './AirbnbStyleGuide';
export default AirbnbStyleGuide.es6;

// best
import { es6 } from './AirbnbStyleGuide';
export default es6;
```

## 不要使用通配符导入

> 为什么? 这确保您有一个默认导出。

```javascript
// bad
import * as AirbnbStyleGuide from './AirbnbStyleGuide';

// good
import AirbnbStyleGuide from './AirbnbStyleGuide';
```

## 不要从`import`里面直接使用`export`导出


```javascript
// bad
// filename es6.js
export { es6 as default } from './airbnbStyleGuide';

// good
// filename es6.js
import { es6 } from './AirbnbStyleGuide';
export default es6;
```

