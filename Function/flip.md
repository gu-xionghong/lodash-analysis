# lodash 源码阅读 —— flip

> "Let go of the thoughts that don't make you strong." —Unknown

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * 创建一个函数，调用func时候接收翻转的参数。
 *
 * @since 4.0.0
 * @category Function
 * @param {Function} func 要翻转参数的函数。
 * @returns {Function} 返回新的函数。
 */
function flip(func) {
  if (typeof func != 'function') {
    throw new TypeError('Expected a function');
  }
  return function(...args) {
    return func.apply(this, args.reverse());
  };
}
```

## 原理

使用扩展运算符 `...` 获取参数数组 `args`，再通过 `Array.prototype.reverse` 将参数数组翻转，传给 `func` 作为参数执行。

## 相关链接

- [lodash 技巧 —— 高阶函数](../Tips/higherOrderFunction.md)
- [MDN - 展开语法](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/setTimeout)
