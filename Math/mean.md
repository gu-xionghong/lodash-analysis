# lodash 源码阅读 —— mean

> "Let go of the thoughts that don't make you strong." —Unknown

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import baseMean from './meanBy.js';
```

- [lodash 源码阅读 —— meanBy](../Math/meanBy.md)

## 源码

```js
/**
 * 计算 array 的平均值。
 *
 * @since 4.0.0
 * @category Math
 * @param {Array} array 要迭代的数组。
 * @returns {number} 返回平均值。
 * @example
 *
 * mean([4, 2, 8, 6])
 * // => 5
 */
function mean(array) {
  return baseMean(array, value => value);
}
```

## 原理

传入返回自身的 `iteratee` 给 `meanBy` 方法，计算普通一维数组的平均值。

## 相关链接

- [lodash 源码阅读 —— meanBy](../Math/meanBy.md)
