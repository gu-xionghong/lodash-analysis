# lodash 源码阅读 —— isArrayLike

> "I'm not in this world to live up to your expectations and you're not in this world to live up to mine." —Bruce Lee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import isLength from './isLength.js';
```

- [lodash 源码阅读 —— isLength](../Lang/isLength.md)

## 源码

```js
/**
 * 检查 `value` 是否为类数组。
 * 如果值不是函数并且具有大于或等于 `0` 且小于或等于 `Number.MAX_SAFE_INTEGER` 的整数的 `value.length`，则该值被视为类数组。
 *
 * @since 4.0.0
 * @category Lang
 * @param {*} value 需要检查的值
 * @returns {boolean} 返回布尔值，true 表示类数组，否则为不
 * @example
 *
 * isArrayLike([1, 2, 3])
 * // => true
 *
 * isArrayLike(document.body.children)
 * // => true
 *
 * isArrayLike('abc')
 * // => true
 *
 * isArrayLike(Function)
 * // => false
 */
function isArrayLike(value) {
  return value != null && typeof value != 'function' && isLength(value.length);
}
```

## 相关链接

- [lodash 源码阅读 —— isLength](../Lang/isLength.md)
