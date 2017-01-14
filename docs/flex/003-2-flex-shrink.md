# flex-shrink

`flex-shrink`属性和`flex-grow`相反，默认值为`0`,当`flex-container`空间就算不够时，也不允许缩小，当`flex-shrink`的值为非`0`的正数时，表示当前`flex-item`相对与其他的`兄弟item`的缩小比例值。

### Value

```js

.flex-item {
  -webkit-flex-shrink: <number>; /* Safari */
  flex-shrink: <number>;
}

```

假设下图中除了`2`的`flex-shrink`值为默认值`0`，其他的都为`1`，那么当空间不足时，`2`并不会变小，其它的兄弟视图等比例缩小。

[![cover](http://ojp7xe8x3.bkt.clouddn.com/flexbox-flex-shrink.jpg)](http://ojp7xe8x3.bkt.clouddn.com/flexbox-flex-shrink.jpg)


Default value: `1`
