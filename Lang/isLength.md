# lodash 源码阅读 —— isLength

> "I'm not in this world to live up to your expectations and you're not in this world to live up to mine." —Bruce Lee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
const MAX_SAFE_INTEGER = 9007199254740991;

/**
 * 检查 `value` 是否是一个有效的数组长度。
 *
 * 注意： 此方法基于 [`ToLength`](http://ecma-international.org/ecma-262/7.0/#sec-tolength).
 *
 * @since 4.0.0
 * @category Lang
 * @param {*} value The value to check.
 * @returns {boolean} Returns `true` if `value` is a valid length, else `false`.
 * @example
 *
 * isLength(3)
 * // => true
 *
 * isLength(Number.MIN_VALUE)
 * // => false
 *
 * isLength(Infinity)
 * // => false
 *
 * isLength('3')
 * // => false
 */
function isLength(value) {
  return typeof value == 'number' && value > -1 && value % 1 == 0 && value <= MAX_SAFE_INTEGER;
}
```

## 知识点

在 [ES spec - ToLength](http://ecma-international.org/ecma-262/9.0/#sec-tolength) 中我们了解到，有效的 类数组 长度值需要具备以下条件：

1. 数值类型且为整数
2. 大于或等于 `0`
3. 小于或等于 `Number.MAX_SAFE_INTEGER`

## 相关链接

- [ES spec - ToLength](http://ecma-international.org/ecma-262/9.0/#sec-tolength)
