# Flex布局

网页布局（layout）是CSS的一个重点应用。

## 传统布局

[![cover](http://ojp7xe8x3.bkt.clouddn.com/chuantong.gif)](http://ojp7xe8x3.bkt.clouddn.com/chuantong.gif)

布局的传统解决方案，基于盒状模型，依赖 display属性 + position属性 + float属性。

## Flexbox 布局
[![cover](http://ojp7xe8x3.bkt.clouddn.com/flexbox.png)](http://ojp7xe8x3.bkt.clouddn.com/flexbox.png)
CSS Flexible Box Layout Module 简称 Flexbox Layout，Flexbox 布局是CSS3中一种新的布局模式，用于改进传统模式中标签对齐、方向、以及排序等等缺陷。

The prime characteristic of the flex container is the ability to modify the width or height of its children to fill the available space in the best possible way on different screen sizes.

最重要的特点是当父视图因为不同的屏幕而改变自身大小时，父视图可以动态的改变子视图的宽和高来尽可能的填充父视图可用的空间。

许多设计师和开发者发现flexbox布局更容易使用，元素的定位相对于传统布局只需要使用更少的代码即可实现，使开发过程更简单。

## 最新的flexbox支持的浏览器

- Chrome 29+
- Firefox 28+
- Internet Explorer 11+
- Opera 17+
- Safari 6.1+ (prefixed with -webkit-)
- Android 4.4+
- iOS 7.1+ (prefixed with -webkit-)

[查看浏览器支持特性](http://caniuse.com/#feat=flexbox).


## flexbox用法
要想使用 flexbox 布局只需要在父标签设置display属性即可:

```javascript
.flex-container {
  display: -webkit-flex; /* Safari */
  display: flex;
}
```

如果你希望你的子元素能够使用flexbox布局，你可以这样写：

```javascript
.flex-container {
  display: -webkit-inline-flex; /* Safari */
  display: inline-flex;
}
```

`Note`:这是让container能够使用flexbox布局的唯一属性，它能够让所有的子视图立刻变成flex items。

