# lodash 源码阅读 —— inRange

> "Dude, suckin' at something is the first step to being sorta good at something." —Jake, "Adventure Time"

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import baseInRange from './.internal/baseInRange.js';
```

- [lodash 源码阅读 —— baseInRange](../Internal/baseInRange.md)

## 源码

```js
/**
 * 检查 number 是否在 start 与 end 之间，但不包括 end。 如果 end 没有指定，那么 start 设置为0。 如果 start 大于 end，那么参数会交换以便支持负范围。
 *
 * @since 3.3.0
 * @category Number
 * @param {number} number 要检查的值。
 * @param {number} [start=0] 开始范围。
 * @param {number} end 结束范围。
 * @returns {boolean} 如果number在范围内 ，那么返回true，否则返回 false。
 */
function inRange(number, start, end) {
  if (end === undefined) {
    end = start;
    start = 0;
  }
  return baseInRange(+number, +start, +end);
}
```

## 原理

`inRange` 主要对 `baseInRange` 多做了一个功能，便是判断 `end` 是否存在，如果 `end` 没有指定，那么 `start` 设置为 0，后将参数 `Number` 化传递给 [baseInRange](../Internal/baseInRange.md) 进行区间判断。

## 相关链接

- [lodash 源码阅读 —— baseInRange](../Internal/baseInRange.md)
