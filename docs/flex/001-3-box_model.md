# 盒子模型

Before we start with describing the flexbox properties let’s give a little introduction of the flexbox model. The flex layout is constituted of parent container referred as flex container and its immediate children which are called flex items.

在我们开始学习flexbox相关属性之前，我们先介绍一下flexbox model。
## 类和对象的类比
类：它是抽象的概念，比如`div`，`p`，`span`，`input`等等标签

对象：对象是具体的东西，比如`<div></div>`,`<p />`,`<span></span>`, `<input />`等等


## 盒子模型结构

### 代码

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>flexbox-model</title>
    <style>

        .flexbox-model-container {
            background-color: #FECE3F;
            width: 600px;
            height: 250px;
            display: flex;
        }

        .flexbox-model {
            background-color: green;
            width: 200px;
            height: 50px;
            padding: 50px;
            border: 10px solid red;
            margin: 20px;
        }
    </style>
</head>
<body>

<div class="flexbox-model-container">
    <div class="flexbox-model">
        flexbox-model
    </div>
</div>

</body>
</html>
```


### 效果图

[![cover](http://ojp7xe8x3.bkt.clouddn.com/Snip20170114_38.png)](http://ojp7xe8x3.bkt.clouddn.com/Snip20170114_38.png)

### width和height计算
盒子的宽度 = 效果图中蓝色边框的宽度

盒子的高度 = 效果图中蓝色边框的高度

### 标准的盒子模型结构图
[![cover](http://ojp7xe8x3.bkt.clouddn.com/biaozhunboxmodel.png)](http://ojp7xe8x3.bkt.clouddn.com/biaozhunboxmodel.png)

## flex-container和flex-item之间的关系

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>flexbox-model</title>
    <style>
        .flex-item {
            width: 120px;
            height: 120px;
            background-color: white;
            margin: 20px;
        }

        .flex-container {
            background-color: #FECE3F;
            width: 600px;
            height: 220px;
            display: flex;
            justify-content: center;
            align-items: center;
        }
    </style>
</head>
<body>

<div class="flex-container">
    <div class="flex-item">flex-item</div>
    <div class="flex-item">flex-item</div>
    <div class="flex-item">flex-item</div>
</div>

</body>
</html>

```

## 效果图解析
下图中黄色的图是`flex-container`,三个白色的正方形是`flex-item`,`flex-container`是`flex-item`的父视图，我们通常叫`容器`,`flex-item`是`flex-container`的子视图，我们通常叫做`项目`,`容器`中可以有多个`项目`，一个`项目`只有一个直接的`容器`，`容器`里面的多个`项目`有排列方向，下图中，三个项目的排列方向是从左到右排列，我们把和排列方向一致的这条线叫做`主轴`，另外一条线叫做`交叉轴`.

[![cover](http://ojp7xe8x3.bkt.clouddn.com/CSS3-Flexbox-Model.jpg)](http://ojp7xe8x3.bkt.clouddn.com/CSS3-Flexbox-Model.jpg)

## 容器的flexbox属性
- flex-direction
- flex-wrap
- flex-flow
- justify-content
- align-items
- align-content

## 项目的flexbox属性
- order
- flex-grow
- flex-shrink
- flex-basis
- flex
- align-self
