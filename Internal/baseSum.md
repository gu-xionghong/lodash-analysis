# lodash 源码阅读 —— baseSum

> "Let go of the thoughts that don't make you strong." —Unknown

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * sum 跟 sumBy 的基础实现
 *
 * @private
 * @param {Array} array 要迭代的数组。
 * @param {Function} iteratee 调用每个元素的迭代函数。
 * @returns {number} 返回总和。
 */
function baseSum(array, iteratee) {
  let result;

  for (const value of array) {
    const current = iteratee(value);
    if (current !== undefined) {
      result = result === undefined ? current : result + current;
    }
  }
  return result;
}
```

## 原理

`baseSum` 使用 `for...of` 遍历 `array`，这样做的好处是可以兼容自定义 `Symbol.iterator` 的 `array` 参数以及更多的类型（例如: `String/TypeArray/Map/Set/arguments对象/DOM集合`等）；再通过传入的 `iteratee` 方法取得当前值 `current`，通过对 `current` 的类型判断，排除 `null/undefinded/NaN/Symbol` 情况，将合法的 `current` 计入 `result` 中，最终计算返回正确的总值结果。

## Fix

阅读版本代码存在错误，已提 [PR](https://github.com/lodash/lodash/pull/4233) 进行修复.

## 相关链接

- [MDN - for...of](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of)
- [lodash 源码阅读 —— isSymbol](../Lang/isSymbol.md)
