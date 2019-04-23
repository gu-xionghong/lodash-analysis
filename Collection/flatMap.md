# lodash 源码阅读 —— flatMap

> "It is our choices that show what we truly are, far more than our abilities." —@@jk_rowling

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import baseFlatten from './.internal/baseFlatten.js';
import map from './map.js';
```

- [lodash 源码阅读 —— map](../Array/map.md)
- [lodash 源码阅读 —— baseFlatten](../Internal/baseFlatten.md)

## 源码

```js
/**
 * 创建一个扁平化的数组
 * 这个数组的值来自collection（集合）中的每一个值经过 iteratee（迭代函数） 处理后返回的结果，并且扁平化合并。
 * iteratee 调用三个参数： (value, index|key, collection)。
 *
 * @since 4.0.0
 * @category Collection
 * @param {Array|Object} collection 迭代遍历的集合。
 * @param {Function} iteratee 每次迭代调用的函数。
 * @returns {Array} 返回新扁平化数组。
 */
function flatMap(collection, iteratee) {
  // 使用 map 遍历 collection 返回新的集合，再使用 baseFlatten 进行扁平化操作
  return baseFlatten(map(collection, iteratee), 1);
}
```

## 相关链接

- [lodash 源码阅读 —— map](../Array/map.md)
- [lodash 源码阅读 —— baseFlatten](../Internal/baseFlatten.md)
