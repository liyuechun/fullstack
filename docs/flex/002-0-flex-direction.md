# flex-direction
CSS flex-direction 属性指定了内部元素是如何在 flex 容器中布局的，定义了主轴的方向(正方向或反方向)。

请注意，值 row 和 row-reverse 受 flex 容器的方向性的影响。 如果它的 dir 属性是 ltr，row 表示从左到右定向的水平轴，而 row-reverse 表示从右到左; 如果 dir 属性是 rtl，row 表示从右到左定向的轴，而 row-reverse 表示从左到右。

## row
flex容器的主轴被定义为与文本方向相同。 主轴起点和主轴终点与内容方向相同。

### Value

```JavaScript
.flex-container {
  -webkit-flex-direction: row; /* Safari */
  flex-direction: row;
}
```

### 效果图
[![cover](http://ojp7xe8x3.bkt.clouddn.com/flexbox-flex-direction-row.jpg)](http://ojp7xe8x3.bkt.clouddn.com/flexbox-flex-direction-row.jpg)


## row-reverse
表现和row相同，但是置换了主轴起点和主轴终点
### Value

```JavaScript
.flex-container {
  -webkit-flex-direction: row-reverse; /* Safari */
  flex-direction: row-reverse;
}
```

### 效果图
[![cover](http://ojp7xe8x3.bkt.clouddn.com/flexbox-flex-direction-row-reverse.jpg)](http://ojp7xe8x3.bkt.clouddn.com/flexbox-flex-direction-row-reverse.jpg)

## column
flex容器的主轴和块轴相同。主轴起点与主轴终点和书写模式的前后点相同
### Value

```JavaScript
.flex-container {
  -webkit-flex-direction: column; /* Safari */
  flex-direction: column;
}
```

### 效果图
[![cover](http://ojp7xe8x3.bkt.clouddn.com/flexbox-flex-direction-column.jpg)](http://ojp7xe8x3.bkt.clouddn.com/flexbox-flex-direction-column.jpg)

## column-reverse
表现和column相同，但是置换了主轴起点和主轴终点.
### Value

```JavaScript
.flex-container {
  -webkit-flex-direction: column-reverse; /* Safari */
  flex-direction: column-reverse;
}
```

### 效果图
[![cover](http://ojp7xe8x3.bkt.clouddn.com/flexbox-flex-direction-column-reverse.jpg)](http://ojp7xe8x3.bkt.clouddn.com/flexbox-flex-direction-column-reverse.jpg)


Default value: `row`
