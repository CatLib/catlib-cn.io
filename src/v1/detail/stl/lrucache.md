---
title: 近期最少使用缓存
type: detail
order: 102
enable: true
---

## 近期最少使用缓存

近期最少使用缓存根据数据的历史访问记录来进行淘汰数据。当链表满的时候，将链表尾部的数据丢弃。

> 当存在热点数据时，LRU的效率很好，批量操作会导致LRU命中率急剧下降，缓存污染情况比较严重。所以这个结构仅适合简单的缓存。

### 可用方法

- [Add](#Add)
- [Count](#Count)
- [Get](#Get)
- [Remove](#Remove)

### Add

添加一个`element`到近期最少使用缓存，或者如果它已经存在则将其移动到缓存头部。

``` csharp
lruCache.Add("key" , "val");
```

### Count

获取Lru缓存中的`element`数量。

``` csharp
var num = lruCache.Count;
```

### Get

根据`key`获取`element`，如果被淘汰则返回传入的`默认值`。

``` csharp
var element = lruCache.Get("key" , "default val");
```

### Remove

根据`key`移除`element`。

``` csharp
lruCache.Remove("key");
```