# http缓存

前后端的http交互中，使用缓存能很大程度上的提升效率，而且基本上对性能有要求的前端项目都是必用缓存的

## 强缓存与弱缓存

- 缓存可以简单的划分成两种类型：强缓存（200 from cache）与协商缓存（304）

区别简述如下：

- - 强缓存（200 from cache）时，浏览器如果判断本地缓存未过期，就直接使用，无需发起http请求

- - 协商缓存（304）时，浏览器会向服务端发起http请求，然后服务端告诉浏览器文件未改变，让浏览器使用本地缓存对于协商缓存，使用Ctrl + F5强制刷新可以使得缓存无效

但是对于强缓存，在未过期时，必须更新资源路径才能发起新的请求（更改了路径相当于是另一个资源了，这也是前端工程化中常用到的技巧）

![区别图](https://github.com/ChesterBu/Blog/blob/master/img/netImg/cache1.png?raw=true)

## 缓存头部关系

- [各种缓存头部详解](http://imweb.io/topic/5795dcb6fb312541492eda8c)

![关系图](https://github.com/ChesterBu/Blog/blob/master/img/netImg/cache2.png?raw=true)
