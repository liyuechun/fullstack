# flex-grow
`flex-grow`属性的默认值为`0`,当它为0时，尽管`flex-container`剩余很多多余的空间，但是当前的`flex-item`并不会自动伸缩以填充`flex-container`多余的空间。

其实我们可以这么总结，`flex-grow`属性值决定它相对与其他兄弟视图自动填充`flex-container`剩余空间的比例。

### Values

```js

.flex-item {
  -webkit-flex-grow: <number>; /* Safari */
  flex-grow:         <number>;
}

```
当所有的`item`的`flex-grow`的值相同时，他们所占据的空间相同。
[![cover](http://ojp7xe8x3.bkt.clouddn.com/flexbox-flex-grow-1.jpg)](http://ojp7xe8x3.bkt.clouddn.com/flexbox-flex-grow-1.jpg)

下图中5个`flex-item`的比例关系为:`1:3:1:1:1`

[![cover](http://ojp7xe8x3.bkt.clouddn.com/flexbox-flex-grow-2.jpg)](http://ojp7xe8x3.bkt.clouddn.com/flexbox-flex-grow-2.jpg)

Default value: `Default value: 0`
