# lodash 源码阅读 —— maxBy

> "Let go of the thoughts that don't make you strong." —Unknown

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import isSymbol from './isSymbol.js';
```

- [lodash 源码阅读 —— isSymbol](../Lang/isSymbol.md)

## 源码

```js
/**
 * 这个方法类似 _.max(已弃用), 计算 array 中的最大值， 如果 array 是 空的或者假值将会返回 undefined
 * 但它接受 iteratee 来调用 array 中的每一个元素，来生成其值排序的标准。
 * iteratee 会调用1个参数: (value) 。
 *
 * @since 4.0.0
 * @category Math
 * @param {Array} array 要迭代的数组。
 * @param {Function} iteratee 调用每个元素的迭代函数。
 * @returns {*} 返回最大的值。
 * @example
 *
 * const objects = [{ 'n': 1 }, { 'n': 2 }]
 *
 * maxBy(objects, ({ n }) => n)
 * // => { 'n': 2 }
 */
function maxBy(array, iteratee) {
  let result;
  if (array == null) {
    return result;
  }
  let computed;
  for (const value of array) {
    const current = iteratee(value);

    if (current != null && (computed === undefined ? current === current && !isSymbol(current) : current > computed)) {
      computed = current;
      result = value;
    }
  }
  return result;
}
```

## 原理

`maxBy` 使用 `for...of` 遍历 `array`，这样做的好处是可以兼容自定义 `Symbol.iterator` 的 `array` 参数以及更多的类型（例如: `String/TypeArray/Map/Set/arguments对象/DOM集合`等）；再通过传入的 `iteratee` 方法取得当前值 `current`，通过对 `current` 的类型判断，排除 `null/undefinded/NaN/Symbol` 情况，与缓存变量 `computed` 比对大小，最终计算返回正确的最大值结果。

## Fix

阅读版本代码存在错误，已提 [PR](https://github.com/lodash/lodash/pull/4233) 进行修复.

## 相关链接

- [MDN - for...of](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of)
- [lodash 源码阅读 —— isSymbol](../Lang/isSymbol.md)
