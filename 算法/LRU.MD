### 什么是LRU，LRU缓存淘汰策略

缓存在计算机网络上随处可见，例如：当我们首次访问一个网页时，打开很慢，但当我们再次打开这个网页时，打开就很快。

这就涉及缓存在浏览器上的应用：浏览器缓存。当我们打开一个网页时，例如 https://github.com/Aszzo/Daily ，它会在发起真正的网络请求前，查询浏览器缓存，看是否有要请求的文件，如果有，浏览器将会拦截请求，返回缓存文件，并直接结束请求，不会再去服务器上下载。如果不存在，才会去服务器请求。

其实，浏览器中的缓存是一种在本地保存资源副本，它的大小是有限的，当我们请求数过多时，缓存空间会被用满，此时，继续进行网络请求就需要确定缓存中哪些数据被保留，哪些数据被移除，这就是浏览器缓存淘汰策略，最常见的淘汰策略有 FIFO（先进先出）、LFU（最少使用）、LRU（最近最少使用）。

LRU （ Least Recently Used ：最近最少使用 ）缓存淘汰策略，故名思义，就是根据数据的历史访问记录来进行淘汰数据，其核心思想是 如果数据最近被访问过，那么将来被访问的几率也更高 ，优先淘汰最近没有被访问到的数据。

可以用一张图加深理解

![avatar](https://camo.githubusercontent.com/a06af57cca996cf2f3a7b80f1619ed3842c119fd/687474703a2f2f7265736f757263652e6d757969792e636e2f696d6167652f32303230303430363230353234372e706e67)

### leetcode真题

运用你所掌握的数据结构，设计和实现一个 LRU (最近最少使用) 缓存机制。它应该支持以下操作： 获取数据 get 和写入数据 put 。

获取数据 `get(key)` - 如果密钥 `( key )` 存在于缓存中，则获取密钥的值（总是正数），否则返回 -1 。
写入数据 `put(key, value)` - 如果密钥不存在，则写入数据。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据，从而为新数据留出空间。

进阶:

你是否可以在 O(1) 时间复杂度内完成这两种操作？

示例:
```js
LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // 返回  1
cache.put(3, 3);    // 该操作会使得密钥 2 作废
cache.get(2);       // 返回 -1 (未找到)
cache.put(4, 4);    // 该操作会使得密钥 1 作废
cache.get(1);       // 返回 -1 (未找到)
cache.get(3);       // 返回  3
cache.get(4);       // 返回  4
```
附leetcoded地址：[leetcode](https://leetcode-cn.com/problems/lru-cache/)

```js
/**
 * @param {number} capacity
 */
var LRUCache = function (capacity) {
    this.max = capacity;
    this.hash = new Map();
};

/** 
 * @param {number} key
 * @return {number}
 */
LRUCache.prototype.get = function (key) {
    if (this.hash.has(key)) {
        // 要把最近使用的置于栈顶
        const val = this.hash.get(key);
        this.hash.delete(key);
        this.hash.set(key, val)

        return val
    }
    return -1
};

/** 
 * @param {number} key 
 * @param {number} value
 * @return {void}
 */
LRUCache.prototype.put = function (key, value) {
    // 如果之前存在，直接替换
    if (this.hash.has(key)) {
        this.hash.delete(key)
    }
    const size = this.hash.size;

    if (size >= this.max) {
        // 如果此时的队列已满,要删除底部的元素
        const key = this.hash.keys().next().value;
        this.hash.delete(key)
    } 
    this.hash.set(key, value)
    
    
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * var obj = new LRUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */
const cache = new LRUCache(2);
cache.put(1, 1);
cache.put(2, 2);
console.log(cache.get(1));       // 返回  1
cache.put(3, 3);                 // 该操作会使得密钥 2 作废
console.log(cache.get(2));       // 返回 -1 (未找到)
cache.put(4, 4);                 // 该操作会使得密钥 1 作废
console.log(cache.get(1));       // 返回 -1 (未找到)
console.log(cache.get(3));       // 返回  3
console.log(cache.get(4));       // 返回  4
console.log(cache.hash)

```