# lodash 源码阅读 —— sum

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
 * 计算 array 中值的总和
 *
 * @since 3.4.0
 * @category Math
 * @param {Array} array 要迭代的数组。
 * @returns {number} 返回总和。
 * @example
 *
 * sum([4, 2, 8, 6])
 * // => 20
 */
function sum(array) {
  return array != null && array.length ? baseSum(array, value => value) : 0;
}
```

## 原理

判断传入的数组是否为空或者假值，是则返回 `0`，否则通过 `baseSum` 计算 `array` 中值的总和。

## 相关链接

- [lodash 源码阅读 —— sumBy](../Math/sumBy.md)
- [lodash 源码阅读 —— baseSum](../Internal/baseSum.md)
