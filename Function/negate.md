# lodash 源码阅读 —— negate

> "Let go of the thoughts that don't make you strong." —Unknown

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * 创建一个针对断言函数 func 结果取反的函数。 func 断言函数被调用的时候，this 绑定到创建的函数，并传入对应参数。
 *
 * @since 3.0.0
 * @category Function
 * @param {Function} predicate 需要对结果取反的函数。
 * @returns {Function} 返回一个新的取反函数。
 */
function negate(predicate) {
  if (typeof predicate != 'function') {
    throw new TypeError('Expected a function');
  }
  return function(...args) {
    return !predicate.apply(this, args);
  };
}
```

## 原理

通过 `!` 否定符对断言函数 `predicate` 结果进行取反.

## 相关链接

- [lodash 技巧 —— 高阶函数](../Tips/higherOrderFunction.md)
