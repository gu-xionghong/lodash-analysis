# lodash 源码阅读 —— once

> "Let go of the thoughts that don't make you strong." —Unknown

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import before from './before.js';
```

- [lodash 源码阅读 —— before](../Function/before.md)

## 源码

```js
/**
 * 创建一个只能调用 func 一次的函数。 重复调用返回第一次调用的结果。 func 调用时， this 绑定到创建的函数，并传入对应参数。
 *
 * @since 0.1.0
 * @category Function
 * @param {Function} func 指定的触发的函数。
 * @returns {Function} 返回新的受限函数。
 */
function once(func) {
  return before(2, func);
}
```

## 原理

通过 `before` 函数生成只执行一次的函数。

## 相关链接

- [lodash 技巧 —— 高阶函数](../Tips/higherOrderFunction.md)
- [lodash 源码阅读 —— before](../Function/before.md)
