# align-self

`align-self`主要用在当因为`flex-container`上的属性`align-items`属性改变了自己的状态但是又希望自己的状态和其它兄弟视图之间的状态不一样时，就可以使用`align-self`来的自身的状态进行设置。

```js
.flex-item {
  -webkit-align-self: auto | flex-start | flex-end | center | baseline | stretch; /* Safari */
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

[![](http://ojp7xe8x3.bkt.clouddn.com/flexbox-align-self.jpg)](http://ojp7xe8x3.bkt.clouddn.com/flexbox-align-self.jpg)

Default value: `auto`
