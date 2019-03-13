# lodash 源码阅读 —— defer

> "Let go of the thoughts that don't make you strong." —Unknown

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * 推迟调用func，直到当前堆栈清理完毕。 调用时，任何附加的参数会传给func。
 *
 * @since 0.1.0
 * @category Function
 * @param {Function} func 要延迟的函数。
 * @param {...*} [args] 会在调用时传给 func 的参数。
 * @returns {number} 返回计时器 id。
 * @example
 *
 * defer(text => console.log(text), 'deferred')
 * // => Logs 'deferred' after one millisecond.
 */
function defer(func, ...args) {
  if (typeof func != 'function') {
    throw new TypeError('Expected a function');
  }
  return setTimeout(func, 1, ...args);
}
```

## 原理

通过 `setTimeout` 将 `func` 插入到 JavaScript [事件循环](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/EventLoop)的消息队列中，使 `func` 会在当前堆栈清理完毕后被调用。

## 相关链接

- [lodash 技巧 —— 高阶函数](../Tips/higherOrderFunction.md)
- [MDN - window.setTimeout](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/setTimeout)
- [MDN - 并发模型与事件循环](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/EventLoop)
- [stackoverflow - Difference between setTimeout(fn, 0) and setTimeout(fn, 1)?](https://stackoverflow.com/questions/8341803/difference-between-settimeoutfn-0-and-settimeoutfn-1)
