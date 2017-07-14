## 添加一些数据

我最喜欢的MongoDB的特性就是它使用JSON作为数据结构，这就意味着我们对此非常的熟悉。如果你不熟悉JSON，先去读点相关资料吧，这超出了本教程的范围。

我们添加一些数据到collection中。在这个教程里，我们只有一个简单的数据库，username和email两个字段。我们的数据看起来是这个样子的：

```js
{
    "_id" : 1234,
    "username" : "liyuechun",
    "email" : "liyuechun@cldy.org"
}
```
你可以创建你自己的_id字段的值，不过我觉得最好还是让mongo来做这件事。它会为每一条记录创建一个唯一的值。我们看看它是怎么工作的。在mongo的窗口中，输入：

```js
> db.usercollection.insert({"username" : "liyuechun", "email" : "liyuechun@cldy.org" })
WriteResult({ "nInserted" : 1 })
>
```

**重要提示**：db就是我们上面创建的`nodetest1`数据库，就是我们的`collection`，相当于一张数据表。注意我们不需要提前创建这个`collection`，它会在第一次使用的时候自动创建。好了，按下回车。