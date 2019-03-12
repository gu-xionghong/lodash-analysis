# lodash 源码阅读 —— sumBy

> "Let go of the thoughts that don't make you strong." —Unknown

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import baseSum from './.internal/baseSum.js';
```

- [lodash 源码阅读 —— baseSum](../Internal/baseSum.md)

## 源码

```js
/**
 * 这个方法类似 _.sum
 * 但它接受 iteratee 来调用 array 中的每一个元素，来生成其值排序的标准。
 * iteratee 会调用1个参数: (value) 。
 *
 * @since 4.0.0
 * @category Math
 * @param {Array} array 要迭代的数组。
 * @param {Function} iteratee 调用每个元素的迭代函数。
 * @returns {number} 返回总和。
 * @example
 *
 * const objects = [{ 'n': 4 }, { 'n': 2 }, { 'n': 8 }, { 'n': 6 }]
 *
 * sumBy(objects, ({ n }) => n)
 * // => 20
 */
function sumBy(array, iteratee) {
  return array != null && array.length ? baseSum(array, iteratee) : 0;
}
```

## 原理

判断传入的数组是否为空或者假值，是则返回 `0`，否则通过 `baseSum` 计算 `array` 中值的总和。

## 相关链接

- [lodash 源码阅读 —— sum](../Math/sum.md)
- [lodash 源码阅读 —— baseSum](../Internal/baseSum.md)
