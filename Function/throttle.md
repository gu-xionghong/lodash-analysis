# lodash 源码阅读 —— debounce

> "Use only that which works, and take it from any place you can find it." —Bruce Lee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import debounce from './debounce.js';
import isObject from './isObject.js';
```

- [lodash 源码阅读 —— debounce](../Function/debounce.md)
- [lodash 源码阅读 —— isObject](../Lang/isObject.md)

## 源码注释

```js
/**
 * 创建一个节流函数，在 wait 秒内最多执行 func 一次的函数。
 * 该函数提供一个 cancel 方法取消延迟的函数调用以及 flush 方法立即调用。
 * 可以提供一个 options 对象决定如何调用 func 方法， options.leading 与|或 options.trailing 决定 wait 前后如何触发。
 * func 会传入最后一次传入的参数给这个函数。 随后调用的函数返回是最后一次 func 调用的结果。
 *
 * 注意: 如果 leading 和 trailing 都设定为 true 则 func 允许 trailing 方式调用的条件为: 在 wait 期间多次调用。
 *
 * 如果 wait 为 0 并且 leading 为 false, func调用将被推迟到下一个点，类似setTimeout为0的超时。
 *
 * See [David Corbacho's article](https://css-tricks.com/debouncing-throttling-explained-examples/)
 * for details over the differences between `throttle` and `debounce`.
 *
 * @since 0.1.0
 * @category Function
 * @param {Function} func 要节流的函数。
 * @param {number} [wait=0] 需要节流的毫秒。
 * @param {Object} [options={}] 选项对象。
 * @param {boolean} [options.leading=true] 指定调用在节流开始前。
 * @param {boolean} [options.trailing=true] 指定调用在节流结束后。
 * @returns {Function} 返回节流的函数。
 */
function throttle(func, wait, options) {
  let leading = true;
  let trailing = true;

  if (typeof func !== 'function') {
    throw new TypeError('Expected a function');
  }
  if (isObject(options)) {
    leading = 'leading' in options ? !!options.leading : leading;
    trailing = 'trailing' in options ? !!options.trailing : trailing;
  }
  return debounce(func, wait, {
    leading,
    trailing,
    maxWait: wait // debounce 与 throttle 的主要区别，throttle wait ms 内至少执行一次
  });
}
```

## 相关链接

- [Debouncing and Throttling Explained Through Examples](https://css-tricks.com/debouncing-throttling-explained-examples/)
