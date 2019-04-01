# lodash 源码阅读 —— overArgs

> "Use only that which works, and take it from any place you can find it." —Bruce Lee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * 创建一个函数，调用func时参数为相对应的transforms的返回值。
 *
 * @since 4.0.0
 * @category Function
 * @param {Function} func 要包裹的函数。
 * @param {Function[]} [transforms=[identity]] transforms方法数组.
 * @returns {Function} 返回新函数。
 */
function overArgs(func, transforms) {
  const funcsLength = transforms.length;
  return function(...args) {
    let index = -1;
    const length = Math.min(args.length, funcsLength);
    while (++index < length) {
      args[index] = transforms[index].call(this, args[index]);
    }
    return func.apply(this, args);
  };
}
```

## 原理

`overArgs` 将传入的 `args` 参数，按序传给 `transforms` 函数数组执行，并将处理结果组成成新的参数 `args` 数组，传给 `func` 执行。

## 相关链接

- [lodash 技巧 —— 高阶函数](../Tips/higherOrderFunction.md)
