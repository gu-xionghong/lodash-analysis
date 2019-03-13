# lodash 源码阅读 —— after

> "Let go of the thoughts that don't make you strong." —Unknown

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * _.before 的反向函数;此方法创建一个函数，当他被调用n或更多次之后将马上触发func
 *
 * @since 0.1.0
 * @category Function
 * @param {number} n func 方法应该在调用多少次后才执行。
 * @param {Function} func 受限制执行的函数
 * @returns {Function} 返回新的函数。
 */
function after(n, func) {
  if (typeof func != 'function') {
    throw new TypeError('Expected a function');
  }
  return function(...args) {
    if (--n < 1) {
      return func.apply(this, args);
    }
  };
}
```

## 原理

`after` 首先判断传入的 `func` 参数类型是否合法，再通过闭包方式缓存次数变量 `n` 跟 `func`，返回新的函数，在每次执行返回函数，`n` 次数减少，当 `--n < 1` 之后，之后再执行，则会执行 `func` 方法。

```js
const done = after(3, function() {
  console.log('done!');
});

done(); // n = 2
done(); // n = 1
done(); // n = 0 执行 func，输出 "done!"
```

这里有一个注意点是 `--n < 1`，JavaScript 在执行前置递增和递减操作时，变量的值都是在语句被求值以前改变的。(在计算机科学领域，这种 情况通常被称作副效应。)

```js
n = 2;
--n === 1; // true

n = 2;
n-- === 1; // false
```

## 相关链接

- [lodash 技巧 —— 高阶函数](../Tips/higherOrderFunction.md)
- [lodash 源码阅读 —— before](../Function/before.md)
