# lodash 源码阅读 —— isTypedArray

> "This is your life. Do what you love, and do it often." —@Holstee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import getTag from './.internal/getTag.js';
import nodeTypes from './.internal/nodeTypes.js';
import isObjectLike from './isObjectLike.js';
```

- [lodash 源码阅读 —— getTag](../Internal/getTag.md)
- [lodash 源码阅读 —— nodeTypes](../Internal/nodeTypes.md)
- [lodash 源码阅读 —— isObjectLike](../Lang/isObjectLike.md)

## 源码

```js
/** 用于匹配类型化数组的 `toStringTag` 值 */
const reTypedTag = /^\[object (?:Float(?:32|64)|(?:Int|Uint)(?:8|16|32)|Uint8Clamped)\]$/;

/* Node.js helper 引用. */
const nodeIsTypedArray = nodeTypes && nodeTypes.isTypedArray;

/**
 * 检查 value 是否是 TypedArray。
 *
 * @since 3.0.0
 * @category Lang
 * @param {*} value  要检查的值。
 * @returns {boolean}  如果 value 为一个 TypedArray，那么返回 true，否则返回 false。
 * @example
 *
 * isTypedArray(new Uint8Array)
 * // => true
 *
 * isTypedArray([])
 * // => false
 */
// 判断是否处于 nodejs 环境下，是则调用 util.types.isTypedArray 进行类型检测，否则根据 value 的 toStringTag 值判断
const isTypedArray = nodeIsTypedArray
  ? value => nodeIsTypedArray(value)
  : value => isObjectLike(value) && reTypedTag.test(getTag(value));
```

## 相关链接

- [MDN - TypedArray](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypedArray)
- [lodash 源码阅读 —— getTag](../Internal/getTag.md)
- [lodash 源码阅读 —— nodeTypes](../Internal/nodeTypes.md)
- [lodash 源码阅读 —— isObjectLike](../Lang/isObjectLike.md)
