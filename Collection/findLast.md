# lodash 源码阅读 —— findLast

> "It is our choices that show what we truly are, far more than our abilities." —@@jk_rowling

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import findLastIndex from './findLastIndex.js';
import isArrayLike from './isArrayLike.js';
```

- [lodash 源码阅读 —— findLastIndex](../Array/findLastIndex.md)
- [lodash 源码阅读 —— isArrayLike](../Lang/isArrayLike.md)

## 源码

```js
/**
 * 这个方法类似 _.find ，不同之处在于， _.findLas t是从右至左遍历 collection （集合）元素的。
 *
 * @since 2.0.0
 * @category Collection
 * @param {Array|Object} collection 一个用来迭代的集合。
 * @param {Function} predicate 每次迭代调用的函数。
 * @param {number} [fromIndex=collection.length-1]  开始搜索的索引位置。
 * @returns {*} 返回匹配元素，否则返回 undefined。
 */
function findLast(collection, predicate, fromIndex) {
  let iteratee;
  const iterable = Object(collection);
  // 若为非类数组类型，则通过 Object.keys 获取属性名数组，并重新定义 predicate
  if (!isArrayLike(collection)) {
    collection = Object.keys(collection);
    iteratee = predicate;
    predicate = key => iteratee(iterable[key], key, iterable);
  }
  // 通过 findLastIndex 获取查询结果
  const index = findLastIndex(collection, predicate, fromIndex);
  return index > -1 ? iterable[iteratee ? collection[index] : index] : undefined;
}
```

## 相关链接

- [lodash 源码阅读 —— findLastIndex](../Array/findLastIndex.md)
- [lodash 源码阅读 —— isArrayLike](../Lang/isArrayLike.md)
