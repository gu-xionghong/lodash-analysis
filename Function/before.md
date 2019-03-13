# lodash 源码阅读 —— before

> "Let go of the thoughts that don't make you strong." —Unknown

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * 创建一个调用func的函数，通过this绑定和创建函数的参数调用func，调用次数不超过 n 次。
 * 之后再调用这个函数，将返回一次最后调用func的结果。
 *
 * @since 3.0.0
 * @category Function
 * @param {number} n 限制调用func 的次数
 * @param {Function} func 受限制执行的函数
 * @returns {Function} 返回新的函数
 */
function before(n, func) {
  let result;
  if (typeof func != 'function') {
    throw new TypeError('Expected a function');
  }
  return function(...args) {
    if (--n > 0) {
      result = func.apply(this, args);
    }
    if (n <= 1) {
      func = undefined;
    }
    return result;
  };
}
```

## 原理

`before` 首先判断传入的 `func` 参数类型是否合法，再通过闭包方式缓存次数变量 `n` 、执行函数 `func` 以及执行结果 `result`，返回新的函数，在每次执行返回函数，`n` 次数减少，当 `n <= 1` 之后，销毁 `func` 函数，只返回缓存执行结果 `result`

```js
let times = 0;
const newFunc = before(3, function() {
  return ++times;
});

newFunc(); // n = 2, 执行传入func，return 1
newFunc(); // n = 1, 执行传入func，return 2
newFunc(); // n = 1, 不执行 func，return 2
newFunc(); // n = 1, 不执行 func，return 2
```

## 相关链接

- [lodash 技巧 —— 高阶函数](../Tips/higherOrderFunction.md)
- [lodash 源码阅读 —— after](../Function/after.md)
