---
title: TileStrata缓存深入研究
date: 2018-09-14 16:06:32
tags: 
	- GIS
	- Mapnik
	- TileStrata
	- DataCache
---

使用缓存技术可以加快重复浏览效率，随之而来的是数据更新如何更新缓存的问题，针对这个问题，我们做了研究和思考，可能比较粗浅，权当记录。

#### 方案一：不缓存

这样可以解决缓存更新的问题，但是也无法体验缓存带来的效率提升。

#### 方案二：缓存超时失效

tilestrata-disk提供的更新解决方案，是两个参数：
1.maxage参数
maxage参数的意思是，瓦片存在最长时间是多少，单位是秒，超过这个时间之后，如果有请求进来，则会就删除该缓存，并重新缓存。
```javascript
// cache: cache with maxage
server.layer('mylayer').route('tile.png')
    .use(/* some provider */)
    .use(disk.cache({maxage: 3600, dir: './tiles/mylayer'}));
```
2.refreshage参数
这个参数研究了好半天，才了解其意思。如果瓦片存在时间超过了refreshage的时长，如果有请求进来，则会返回之前的瓦片，返回之后删除之前的瓦片，并重新缓存，这样之后的请求拿到的都是新瓦片。官方原话：`you have tile that's old enough it should be regenerated, but it's not old enough to warrant making the user wait for a new tile to be rendered.`

```javascript
.use(disk.cache({
    dir: './tiles/mylayer',
    refreshage: 3600, // 1 hour
    maxage: 3600*24*7 // 1 week
}));
```

tilestrata-disk也提供了通过回调的方式，动态修改缓存位置、以及超时时间等，例如：
```javascript
// cache: cache with maxage function (advanced)
server.layer('mylayer').route('tile.png')
    .use(/* some provider */)
    .use(disk.cache({
        dir: './tiles/mylayer',
        maxage: function(server, req) {
            if (req.z > 15) return 0; // don't cache
            if (req.z > 13) return 3600;
            return 3600*24;
        }
    }));

// cache: custom directory layout
server.layer('mylayer').route('tile.png')
    .use(/* some provider */)
    .use(disk.cache({path: './tiles/{layer}/{z}/{x}/{y}-{filename}'}));

// cache: custom directory layout (via callback)
server.layer('mylayer').route('tile.png')
    .use(/* some provider */)
    .use(disk.cache({path: function(tile) {
        return './tiles/' + tile.layer + '/' + tile.z + '/' /* ... */
    }}));
```

#### 方案三：强制缓存失效
这里介绍一个工具：[TileMantle](https://github.com/naturalatlas/tilemantle)，该工具可以用来提前生成缓存，以及将缓存设置为失效。

[TileStrata官方文档](https://github.com/naturalatlas/tilestrata#rebuilding-the-tile-cache)中提到了这种模式的使用方法，不过还没有尝试。


关于瓦片缓存更新问题，有可能是这三种方案配合使用，达到最终目的。