
## 关于 
本篇文章致力于教会你如何用Node.js来开发应用，过程中会传授你所有所需的“高级”JavaScript知识。Node入门看这篇文章就够了。

## 代码状态

所有代码为春哥亲测，全部正确通过。

## 阅读文章的对象

1.有编程基础
2.想转向Node.js后端的技术爱好者
3.Node.js新手

## 进入正题

### 1.环境安装

请直接移步[Node.js官网](https://nodejs.org/en/),如下图所示，直接点击最新版下载并进行安装。
![](http://osazvg3ch.bkt.clouddn.com/nodejs.png)

Node.js安装完毕后，打开终端，在终端分别输入如下命令，检测是否安装成功。

```js
Last login: Tue Jun 27 09:19:38 on console
liyuechun:~ yuechunli$ node -v
v8.1.3
liyuechun:~ yuechunli$ npm -v
5.0.3
liyuechun:~ yuechunli$ 
```

如果能够正确显示node和npm的版本，说明Node.js安装成功。

### 2."Hello World"

- 第一种输出方式

好了，“废话”不多说了，马上开始我们第一个Node.js应用：“Hello World”。

```js
liyuechun:~ yuechunli$ node
> console.log("Hello World!");
Hello World!
undefined
> console.log("从零到壹全栈部落!");
从零到壹全栈部落!
undefined
> process.exit()
liyuechun:~ yuechunli$ 
```

在终端里面直接输入命令`node`，接下来输入一句`console.log("Hello World!");` ，回车，即可输出`Hello World`。

简单解释一下为什么每一次打印后面都出现了一个`undefined`,原因是因为你输入js代码并按下回车后，node会输出执行完该代码后的返回值，如果没有返回值，就会显示undefined，这个跟Chrome的调试工具相似。

如上代码所示，当输入`process.exit()`并回车时，即可退出`node模式`。

- 第二种输出方式

```js
Last login: Thu Jun 29 18:17:27 on ttys000
liyuechun:~ yuechunli$ ls
Applications		Downloads		Pictures
Creative Cloud Files	Library			Public
Desktop			Movies
Documents		Music
liyuechun:~ yuechunli$ cd Desktop/
liyuechun:Desktop yuechunli$ mkdir nodejs入门
liyuechun:Desktop yuechunli$ pwd
/Users/liyuechun/Desktop
liyuechun:Desktop yuechunli$ cd nodejs入门/
liyuechun:nodejs入门 yuechunli$ pwd
/Users/liyuechun/Desktop/nodejs入门
liyuechun:nodejs入门 yuechunli$ vi helloworld.js
liyuechun:nodejs入门 yuechunli$ cat helloworld.js 
console.log("Hello World!");
liyuechun:nodejs入门 yuechunli$ node helloworld.js 
Hello World!
liyuechun:nodejs入门 yuechunli$ 
```

**命令解释：**
**ls**：查看当前路径下面的文件和文件夹。
**pwd**：查看当前所在路径。
**cd Desktop**：切换到桌面。
**mkdir nodejs入门**：在当前路径下面创建`nodejs入门`文件夹。
**cd nodejs入门**：进入`nodejs入门`文件夹。
**vi helloworld.js**：创建一个`helloworld.js`文件，并在文件里面输入`console.log("Hello World!")`,保存并退出。
**cat helloworld.js**：查看`helloworld.js`文件内容。
**node helloworld.js**：在当前路径下面执行`helloworld.js`文件。

PS：如果对命令行不熟悉的童鞋，可以用其他编辑器创建一个`helloworld.js`文件，在里面输入`console.log("Hello World！")`，将文件保存到桌面，然后打开终端，直接将`helloworld.js`文件拖拽到终端，直接在终端中执行`node helloworld.js`即可在终端输出`Hello World!`。

好吧，我承认这个应用是有点无趣，那么下面我们就来点“干货”。

下面我们将通过[VSCode](https://code.visualstudio.com/)来进行Node.js的编码。

## 一个完整的基于Node.js的web应用

### 1.用例

我们来把目标设定得简单点，不过也要够实际才行：

- 用户可以通过浏览器使用我们的应用。
- 当用户请求http://domain/start时，可以看到一个欢迎页面，页面上有一个文件上传的表单。
- 用户可以选择一个图片并提交表单，随后文件将被上传到http://domain/upload，该页面完成上传后会把图片显示在页面上。

差不多了，你现在也可以去Google一下，找点东西乱搞一下来完成功能。但是我们现在先不做这个。

更进一步地说，在完成这一目标的过程中，我们不仅仅需要基础的代码而不管代码是否优雅。我们还要对此进行抽象，来寻找一种适合构建更为复杂的Node.js应用的方式。

### 2.应用不同模块分析

我们来分解一下这个应用，为了实现上文的用例，我们需要实现哪些部分呢？

- 我们需要提供Web页面，因此需要一个HTTP服务器
- 对于不同的请求，根据请求的URL，我们的服务器需要给予不同的响应，因此我们需要一个路由，用于把请求对应到请求处理程序（request handler）
- 当请求被服务器接收并通过路由传递之后，需要可以对其进行处理，因此我们需要最终的请求处理程序
- 路由还应该能处理POST数据，并且把数据封装成更友好的格式传递给请求处理入程序，因此需要请求数据处理功能
- 我们不仅仅要处理URL对应的请求，还要把内容显示出来，这意味着我们需要一些视图逻辑供请求处理程序使用，以便将内容发送给用户的浏览器
- 最后，用户需要上传图片，所以我们需要上传处理功能来处理这方面的细节

现在我们就来开始实现之路，先从第一个部分--HTTP服务器着手。