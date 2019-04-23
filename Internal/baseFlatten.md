# lodash 源码阅读 —— flatMap

> "It is our choices that show what we truly are, far more than our abilities." —@@jk_rowling

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import isFlattenable from './isFlattenable.js';
```

- [lodash 源码阅读 —— isFlattenable](../Internal/isFlattenable.md)

## 源码

```js
/**
 * “flatten”的基本实现，支持 restricting flattening.
 *
 * @private
 * @param {Array} array 需要扁平化的数组。
 * @param {number} depth 最大递归深度。
 * @param {boolean} [predicate=isFlattenable] 每次迭代调用的函数。
 * @param {boolean} [isStrict] 限制通过`predicate`检查的值。
 * @param {Array} [result=[]] 初始结果值。
 * @returns {Array} 返回扁平化后的新数组
 */
function baseFlatten(array, depth, predicate, isStrict, result) {
  predicate || (predicate = isFlattenable);
  result || (result = []);

  if (array == null) {
    return result;
  }

  for (const value of array) {
    if (depth > 0 && predicate(value)) {
      if (depth > 1) {
        // Recursively flatten arrays (susceptible to call stack limits).
        baseFlatten(value, depth - 1, predicate, isStrict, result);
      } else {
        result.push(...value);
      }
    } else if (!isStrict) {
      result[result.length] = value;
    }
  }
  return result;
}
```

## 相关链接

- [lodash 源码阅读 —— isFlattenable](../Internal/isFlattenable.md)
