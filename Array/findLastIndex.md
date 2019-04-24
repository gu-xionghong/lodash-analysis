# lodash 源码阅读 —— findLastIndex

> "It is our choices that show what we truly are, far more than our abilities." —@@jk_rowling

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import baseFindIndex from './.internal/baseFindIndex.js';
```

- [lodash 源码阅读 —— baseFindIndex](../Internal/baseFindIndex.md)

## 源码

```js
/**
 * 这个方式类似 _.findIndex， 区别是它是从右到左的迭代集合array中的元素。
 *
 * @since 2.0.0
 * @category Array
 * @param {Array} array 要搜索的数组。
 * @param {Function} predicate 每次迭代调用的函数。
 * @param {number} [fromIndex=array.length-1] 开始搜索的索引位置。
 * @returns {number} 返回找到元素的 索引值（index），否则返回 -1。
 */
function findLastIndex(array, predicate, fromIndex) {
  // 验证 array 合法性
  const length = array == null ? 0 : array.length;
  if (!length) {
    return -1;
  }
  let index = length - 1;
  // 验证 fromIndex 合法性
  if (fromIndex !== undefined) {
    index = fromIndex < 0 ? Math.max(length + fromIndex, 0) : Math.min(fromIndex, length - 1);
  }
  // 调用 baseFindIndex 获取逆向查询结果
  return baseFindIndex(array, predicate, index, true);
}
```

## 相关链接

- [lodash 源码阅读 —— baseFindIndex](../Internal/baseFindIndex.md)
