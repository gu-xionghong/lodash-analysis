# lodash 源码阅读 —— isArguments

> "This is your life. Do what you love, and do it often." —@Holstee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import getTag from './.internal/getTag.js';
import isObjectLike from './isObjectLike.js';
```

- [lodash 源码阅读 —— getTag](../Internal/getTag.md)
- [lodash 源码阅读 —— isObjectLike](../Lang/isObjectLike.md)

## 源码

```js
/**
 * 检查 value 是否是一个类 arguments 对象。
 *
 * @since 0.1.0
 * @category Lang
 * @param {*} value 要检查的值。
 * @returns {boolean} 如果value是一个 arguments 对象 返回 true，否则返回 false。
 * @example
 *
 * isArguments(function() { return arguments }())
 * // => true
 *
 * isArguments([1, 2, 3])
 * // => false
 */
function isArguments(value) {
  // 使用 getTag 获取 value 的 tag 标记
  return isObjectLike(value) && getTag(value) == '[object Arguments]';
}
```

## 相关链接

- [lodash 源码阅读 —— getTag](../Internal/getTag.md)
- [lodash 源码阅读 —— isObjectLike](../Lang/isObjectLike.md)
