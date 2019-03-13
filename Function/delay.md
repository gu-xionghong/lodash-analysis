# lodash 源码阅读 —— delay

> "Let go of the thoughts that don't make you strong." —Unknown

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * 延迟 wait 毫秒后调用 func。 调用时，任何附加的参数会传给func。
 *
 * @since 0.1.0
 * @category Function
 * @param {Function} func 要延迟的函数。
 * @param {number} wait 要延迟的毫秒数。
 * @param {...*} [args] 会在调用时传入到 func 的参数。
 * @returns {number} 返回计时器 id
 */
function delay(func, wait, ...args) {
  if (typeof func != 'function') {
    throw new TypeError('Expected a function');
  }
  return setTimeout(func, +wait || 0, ...args);
}
```

## 原理

通过 `setTimeout` 将延时执行`func`。

## 相关链接

- [lodash 技巧 —— 高阶函数](../Tips/higherOrderFunction.md)
- [MDN - window.setTimeout](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/setTimeout)
