# lodash 源码阅读 —— baseFindIndex

> "It is our choices that show what we truly are, far more than our abilities." —@@jk_rowling

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * `findIndex` 与 `findLastIndex` 的基础实现。
 *
 * @private
 * @param {Array} array 要搜索的数组。
 * @param {Function} predicate 每次迭代调用的函数。
 * @param {number} fromIndex 开始搜索的索引位置。
 * @param {boolean} [fromRight] 制定遍历顺序是否从右往左。
 * @returns {number} 返回找到元素的 索引值（index），否则返回 -1。
 */
function baseFindIndex(array, predicate, fromIndex, fromRight) {
  const { length } = array;
  let index = fromIndex + (fromRight ? 1 : -1);

  // 遍历迭代 array
  while (fromRight ? index-- : ++index < length) {
    if (predicate(array[index], index, array)) {
      return index;
    }
  }
  return -1;
}
```
