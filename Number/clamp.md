# lodash 源码阅读 —— clamp

> "Dude, suckin' at something is the first step to being sorta good at something." —Jake, "Adventure Time"

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * 返回限制在 lower 和 upper 之间的值。
 *
 * @since 4.0.0
 * @category Number
 * @param {number} number 被限制的值。
 * @param {number} lower 下限。
 * @param {number} upper 上限。
 * @returns {number} 返回限制在区域间的值。
 * @example
 *
 * clamp(-10, -5, 5)
 * // => -5
 *
 * clamp(10, -5, 5)
 * // => 5
 */
function clamp(number, lower, upper) {
  number = +number;
  lower = +lower;
  upper = +upper;
  lower = lower === lower ? lower : 0;
  upper = upper === upper ? upper : 0;
  if (number === number) {
    number = number <= upper ? number : upper;
    number = number >= lower ? number : lower;
  }
  return number;
}
```

## 原理

这部分代码不复杂，首先对三个参数做了 `Number` 化，后通过对比值大小，返回限制在区域间的值。  
这里有两个注意的点：

1. 方法直接使用 `+value` 转化参数，如果传入的是 `Symbol` 类值，会导致方法报错，因此我提了个 [ISSUE](https://github.com/lodash/lodash/issues/4227)，理由是为了精简代码，减少依赖，类型问题交给 TypeScript 这样的工具来处理~
2. `value === +value`，这种写法是为了屏蔽掉 `NaN` 的情况：

```js
everyType === NaN;
// false
```
