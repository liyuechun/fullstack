## 数据库查询

```js
> db.usercollection.find().pretty()
{
	"_id" : ObjectId("59659d8fbd3dbae3899471c2"),
	"username" : "liyuechun",
	"email" : "liyuechun@cldy.org"
}
> 
```

当然，你得到ObjectID应该是不一样的，mongo会自动生成一个。如果你以前使用过JSON接口的服务，现在是不是会觉得，哇，在web里调用这个应该很简单吧！嗯，你说对了。

提示：作为正式服务，你应该不希望所有的数据都存在最顶层。关于mongodb数据结构的设计，多看看Google吧。

现在我们有了一条数据，我们多添加点吧。

```js
> db.usercollection.insert(
[
    {"username":"yanan","email":"yanan@cldy.org"},
    {"username":"fujinliang","email":"fujinliang@cldy.org"},
    {"username":"shenpingping","email":"shenpingping@cldy.org"}
])
BulkWriteResult({
	"writeErrors" : [ ],
	"writeConcernErrors" : [ ],
	"nInserted" : 3,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
})
> db.usercollection.find().pretty()
{
	"_id" : ObjectId("59659d8fbd3dbae3899471c2"),
	"username" : "liyuechun",
	"email" : "liyuechun@cldy.org"
}
{
	"_id" : ObjectId("59659ffebd3dbae3899471c3"),
	"username" : "yanan",
	"email" : "yanan@cldy.org"
}
{
	"_id" : ObjectId("59659ffebd3dbae3899471c4"),
	"username" : "fujinliang",
	"email" : "fujinliang@cldy.org"
}
{
	"_id" : ObjectId("59659ffebd3dbae3899471c5"),
	"username" : "shenpingping",
	"email" : "shenpingping@cldy.org"
}
>
```