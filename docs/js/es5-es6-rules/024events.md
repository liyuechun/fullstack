# 事件(Events)

将数据有效内容附加到事件（无论是DOM事件还是像Backbone事件一样更专有的东西），都会传递一个哈希而不是一个原始值。 这允许随后的贡献者向事件有效载荷添加更多的数据，而无需查找和更新事件的每个处理程序。 例如：


```javascript
// bad
$(this).trigger('listingUpdated', listing.id);

...

$(this).on('listingUpdated', function (e, listingId) {
    // do something with listingId
});

```

这个更好:

```javascript
// good
$(this).trigger('listingUpdated', {listingId: listing.id});

...

$(this).on('listingUpdated', function (e, data) {
    // do something with data.listingId
});
```

