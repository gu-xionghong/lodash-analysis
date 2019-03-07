# lodash 源码阅读 —— baseInRange

> "Dude, suckin' at something is the first step to being sorta good at something." —Jake, "Adventure Time"

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * `inRange`基础实现，不对参数做限制
 * 检查 number 是否在 start 与 end 之间，但不包括 end。
 *
 * @private
 * @param {number} number 要检查的值。
 * @param {number} start 开始范围。
 * @param {number} end 结束范围。
 * @returns {boolean} 如果number在范围内 ，那么返回true，否则返回 false。
 */
function baseInRange(number, start, end) {
  return number >= Math.min(start, end) && number < Math.max(start, end);
}
```

## 原理

`baseInRange` 的写法有两个优点：

1. 支持负范围，当 `start` 小于 `end` 时，`baseInRange` 依旧可以正确判断 number 是否在负区间内
2. 兼容 `NaN` 情况，若参数有一个值为 `NaN`，方法将返回 `false`

## 相关链接

- [lodash 源码阅读 —— inRange](../Number/inRange.md)
