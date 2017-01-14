# align-content
当`flex items`在主轴上有`多排(只有一排时此属性不起作用)`时，`align-content`属性主要用于设置`交叉轴`上`flex items`的排列方式。

## stretch
### Value

```JavaScript
.flex-container {
  -webkit-align-items: stretch; /* Safari */
  align-items: stretch;
}
```

### 效果图
[![cover](http://ojp7xe8x3.bkt.clouddn.com/flexbox-align-content-stretch.jpg)](http://ojp7xe8x3.bkt.clouddn.com/flexbox-align-content-stretch.jpg)

## flex-start
### Value

```JavaScript
.flex-container {
  -webkit-align-items: flex-start; /* Safari */
  align-items: flex-start;
}
```

### 效果图
[![cover](http://ojp7xe8x3.bkt.clouddn.com/flexbox-align-content-flex-start.jpg)](http://ojp7xe8x3.bkt.clouddn.com/flexbox-align-content-flex-start.jpg)

## flex-end
### Value

```JavaScript
.flex-container {
  -webkit-align-items: flex-end; /* Safari */
  align-items: flex-end;
}
```

### 效果图
[![cover](http://ojp7xe8x3.bkt.clouddn.com/flexbox-align-content-flex-end.jpg)](http://ojp7xe8x3.bkt.clouddn.com/flexbox-align-content-flex-end.jpg)

## center
### Value

```JavaScript
.flex-container {
  -webkit-align-items: center; /* Safari */
  align-items: center;
}
```

### 效果图
[![cover](http://ojp7xe8x3.bkt.clouddn.com/flexbox-align-content-center.jpg)](http://ojp7xe8x3.bkt.clouddn.com/flexbox-align-content-center.jpg)

## space-between
### Value

```JavaScript
.flex-container {
  -webkit-align-items: space-between; /* Safari */
  align-items: space-between;
}
```

### 效果图
[![cover](http://ojp7xe8x3.bkt.clouddn.com/flexbox-align-content-space-between.jpg)](http://ojp7xe8x3.bkt.clouddn.com/flexbox-align-content-space-between.jpg)


## space-around
### Value

```JavaScript
.flex-container {
  -webkit-align-items: space-around; /* Safari */
  align-items: space-around;
}
```

### 效果图
[![cover](http://ojp7xe8x3.bkt.clouddn.com/flexbox-align-content-space-around.jpg)](http://ojp7xe8x3.bkt.clouddn.com/flexbox-align-content-space-around.jpg)

Default value: `stretch`
