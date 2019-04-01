# lodash 源码阅读 —— debounce

> "Use only that which works, and take it from any place you can find it." —Bruce Lee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import isObject from './isObject.js';
import root from './.internal/root.js'; // 当前运行环境
```

- [lodash 源码阅读 —— isObject](../Lang/isObject.md)

## 源码注释

```js
/**
 * 创建一个 debounced（防抖动）函数，该函数会从上一次被调用后，延迟 wait 毫秒后调用 func 方法。
 * debounced（防抖动）函数提供一个 cancel 方法取消延迟的函数调用以及 flush 方法立即调用。
 * 可以提供一个 options（选项） 对象决定如何调用 func 方法，options.leading 与|或 options.trailing 决定延迟前后如何触发。
 * func 调用时会传入最后一次提供给 debounced（防抖动）函数 的参数。
 * 后续调用的 debounced（防抖动）函数返回是最后一次 func 调用的结果。
 *
 * 注意: 如果 leading 和 trailing 选项为 true, 则 func 允许 trailing 方式调用的条件为: 在 wait 期间多次调用防抖方法。
 *
 * 如果 wait 为 0 并且 leading 为 false, func调用将被推迟到下一个点，类似setTimeout为0的超时。
 *
 * See [David Corbacho's article](https://css-tricks.com/debouncing-throttling-explained-examples/)
 * for details over the differences between `debounce` and `throttle`.
 *
 * @since 0.1.0
 * @category Function
 * @param {Function} func 要防抖动的函数。
 * @param {number} [wait=0]
 *  需要延迟的毫秒数，如果为空，间隔则为 `requestAnimationFrame`。
 * @param {Object} [options={}] 选项对象。
 * @param {boolean} [options.leading=false]
 *  指定在延迟开始前调用。
 * @param {number} [options.maxWait]
 *  设置 func 允许被延迟的最大值。
 * @param {boolean} [options.trailing=true]
 *  指定在延迟结束后调用。
 * @returns {Function} 返回新的 debounced（防抖动）函数。
 */
function debounce(func, wait, options) {
  let lastArgs, // 最后一次 debounced 返回函数执行时的参数 args
    lastThis, // 最后一次 debounced 返回函数执行时的上下文 this
    maxWait, // func 允许被延迟的最大值。
    result, // 最后一次执行的结果
    timerId, // 定时器id
    lastCallTime; // 最后一次 debounced 返回函数执行时的时间

  let lastInvokeTime = 0; // 最后一次 func 被执行的时间
  let leading = false; // 是否在延迟开始前调用
  let maxing = false; // 是否存在 func 允许被延迟的最大值 maxWait
  let trailing = true; // 是否在延迟结束后调用

  // 通过显示传入 wait=0 绕过 requestAnimationFrame 的使用
  const useRAF = !wait && wait !== 0 && typeof root.requestAnimationFrame === 'function';

  // func 类型判断
  if (typeof func !== 'function') {
    throw new TypeError('Expected a function');
  }

  wait = +wait || 0;

  // 获取配置属性
  if (isObject(options)) {
    leading = !!options.leading;
    maxing = 'maxWait' in options;
    maxWait = maxing ? Math.max(+options.maxWait || 0, wait) : maxWait;
    trailing = 'trailing' in options ? !!options.trailing : trailing;
  }

  /**
   * 调用 func 的函数
   * @param {Number} time 时间戳
   */
  function invokeFunc(time) {
    // 获取最后一次执行时的 参数 args 及 上下文 this
    const args = lastArgs;
    const thisArg = lastThis;

    // 清空缓存
    lastArgs = lastThis = undefined;

    // 记录最后一次执行的时间
    lastInvokeTime = time;

    // 执行 func
    result = func.apply(thisArg, args);

    // 返回运行结果
    return result;
  }

  /**
   * 启动计时器
   * @param {Function} pendingFunc 待执行的函数
   * @param {Number} wait 延迟时间
   */
  function startTimer(pendingFunc, wait) {
    // 如果需要使用 requestAnimationFrame，则使用 requestAnimationFrame 进行延时执行
    if (useRAF) {
      root.cancelAnimationFrame(timerId);
      return root.requestAnimationFrame(pendingFunc);
    }
    // 否则使用 setTimeout 延时 wait ms 执行
    return setTimeout(pendingFunc, wait);
  }

  // 取消定时器
  function cancelTimer(id) {
    if (useRAF) {
      return root.cancelAnimationFrame(id);
    }
    clearTimeout(id);
  }

  // 延迟 leading 边缘检测
  function leadingEdge(time) {
    // 重置最后一次执行时间，达到重置 `maxWait` 功能效果
    lastInvokeTime = time;
    // 执行定时器，检测 trailing 后置操作是否需要被执行
    timerId = startTimer(timerExpired, wait);
    // 执行 leading 前置执行
    return leading ? invokeFunc(time) : result;
  }

  // 计算剩余等待时间
  function remainingWait(time) {
    // 距离 debounced 最后一次被调用的时间
    const timeSinceLastCall = time - lastCallTime;
    // 距离 func 最后一次执行的时间
    const timeSinceLastInvoke = time - lastInvokeTime;
    // 剩余等待时间
    const timeWaiting = wait - timeSinceLastCall;

    // 根据 maxWait 情况，返回剩余等待时间
    return maxing ? Math.min(timeWaiting, maxWait - timeSinceLastInvoke) : timeWaiting;
  }

  // 判断是否需要执行 func
  function shouldInvoke(time) {
    // 距离 debounced 最后一次被调用的时间
    const timeSinceLastCall = time - lastCallTime;
    // 距离 func 最后一次执行的时间
    const timeSinceLastInvoke = time - lastInvokeTime;

    // Either this is the first call, activity has stopped and we're at the
    // trailing edge, the system time has gone backwards and we're treating
    // it as the trailing edge, or we've hit the `maxWait` limit.
    return lastCallTime === undefined || timeSinceLastCall >= wait || timeSinceLastCall < 0 || (maxing && timeSinceLastInvoke >= maxWait);
  }

  function timerExpired() {
    const time = Date.now();
    if (shouldInvoke(time)) {
      return trailingEdge(time);
    }
    // 重置定时器
    timerId = startTimer(timerExpired, remainingWait(time));
  }

  // 延迟 trailing 边缘检测
  function trailingEdge(time) {
    timerId = undefined;

    // Only invoke if we have `lastArgs` which means `func` has been
    // debounced at least once.
    if (trailing && lastArgs) {
      return invokeFunc(time);
    }
    lastArgs = lastThis = undefined;
    return result;
  }

  // 取消 debounced，重置所有参数
  function cancel() {
    if (timerId !== undefined) {
      cancelTimer(timerId);
    }
    lastInvokeTime = 0;
    lastArgs = lastCallTime = lastThis = timerId = undefined;
  }

  // flush 立即执行 func
  function flush() {
    return timerId === undefined ? result : trailingEdge(Date.now());
  }

  // 判断是否在定时器倒计时 pending 过程
  function pending() {
    return timerId !== undefined;
  }

  function debounced(...args) {
    const time = Date.now();
    const isInvoking = shouldInvoke(time);

    lastArgs = args;
    lastThis = this;
    lastCallTime = time;

    if (isInvoking) {
      if (timerId === undefined) {
        return leadingEdge(lastCallTime);
      }
      if (maxing) {
        // 存在 maxing 情况，重置定时器并立即执行
        timerId = startTimer(timerExpired, wait);
        return invokeFunc(lastCallTime);
      }
    }
    if (timerId === undefined) {
      timerId = startTimer(timerExpired, wait);
    }
    return result;
  }
  debounced.cancel = cancel;
  debounced.flush = flush;
  debounced.pending = pending;
  return debounced;
}
```

## 相关链接

- [Debouncing and Throttling Explained Through Examples](https://css-tricks.com/debouncing-throttling-explained-examples/)
