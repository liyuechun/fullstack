# CSS介绍

## 什么是层叠样式表
CSS是Cascading Style Sheet（层叠样式表）的缩写。是用于（增强）控制网页样式并允许将样式信息与网页内容分离的一种标记性语言。

## 样式语法

```
Selector {property:value}
```

## 如何将样式表加入您的网页

你可以用以下三种方式将样式表加入您的网页。而最接近目标的样式定义优先权越高。高优先权样式将继承低优先权样式的未重叠定义但覆盖重叠的定义。

### 内联方式 Inline Styles
内联定义即是在对象的标记内使用对象的style属性定义适用其的样式表属性。

```
示例代码：
<p style="color:#f00;">这一行的字体颜色将显示为红色</p>
```

### 内部样式块对象 Embedding a Style Block
你可以在你的HTML文档的<head>标记里插入一个<style>块对象。

```
示例代码：
<style>
body{background:#fff;color:#000;}
p{font-size:14px;}
</style>
```

### 外部样式表 Linking to a Style Sheet
你可以先建立外部样式表文件*.css，然后使用HTML的link对象。

```
示例代码：
<link rel="stylesheet" href="*.css" />
```
