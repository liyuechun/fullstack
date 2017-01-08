# 对象

- pubdate: 2015-04-08

-------

对象包含一个或更多键值对。键的部分可以是任何字符串。值的部分可以是任何类型的值：数字、字符串、数组、函数，甚至是另一个对象。当这些值中的一个是函数，它被称为对象的方法。否则，它们被称为属性。

事实上，JavaScript 中的所有东西几乎都是对象 － 数组，函数，数字，甚至字符串 － 它们都有属性和方法。

```javascript
// Creating an object literal.

var myObject = {
	sayHello: function() {
		console.log( "hello" );
	},
	myName: "Rebecca"
};

myObject.sayHello(); // "hello"

console.log( myObject.myName ); // "Rebecca"
```

创建对象字面量时，注意每一个键值对的键部分可以是任何有效的 JavaScript 标识符、字符串（被引号包含）或者数字：

```javascript
var myObject = {
	validIdentifier: 123,
	"some string": 456,
	99999: 789
};
```

## 遍历对象的可枚举属性：

```javascript
var myObject = {
	validIdentifier: 123,
	"some string": 456,
	99999: 789
};

for ( var prop in myObject ) {
	// Determine if the property is on the object itself.
	// (not on the prototype)
	if ( myObject.hasOwnProperty( prop ) ) {
		console.log( "Property : " + prop + " ; value : " + myObject[ prop ] );
	}
}

// Would log the following:
// Please note that the order is not guaranteed and may differ.

// Property : 99999 ; value : 789
// Property : validIdentifier ; value : 123
// Property : some string ; value : 456
```
### 留言
<div class="ds-thread" data-thread-key="#docs/js/javascript-101/002syntax-basics" data-title="liyuechun.com.cn" data-url="liyuechun.com.cn"></div>

<script type="text/javascript">
var duoshuoQuery = {short_name:"liyuechun"};
	(function() {
		var ds = document.createElement('script');
		ds.type = 'text/javascript';ds.async = true;
		ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
		ds.charset = 'UTF-8';
		(document.getElementsByTagName('head')[0]
		 || document.getElementsByTagName('body')[0]).appendChild(ds);
	})();
	</script>
