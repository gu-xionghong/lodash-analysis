# lodash 源码阅读 —— toFinite

> "Too many of us are not living our dreams because we are living our fears." —@LesBrown77

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import toNumber from './toNumber.js';
```

- [lodash 源码阅读 —— toNumber](../Lang/toNumber.md)

## 源码

```js
const INFINITY = 1 / 0;
const MAX_INTEGER = 1.7976931348623157e308;

/**
 * 将 value 转换为一个有限数字。
 *
 * @since 4.12.0
 * @category Lang
 * @param {*} value 要转换的值。
 * @returns {number} 返回转换后的数字。
 */
function toFinite(value) {
  // falsey 情况下，返回 0
  if (!value) {
    return value === 0 ? value : 0;
  }
  // 转换为 Numebr 类型
  value = toNumber(value);
  // INFINITY、 -INFINITY情况下，转为有限值
  if (value === INFINITY || value === -INFINITY) {
    const sign = value < 0 ? -1 : 1;
    return sign * MAX_INTEGER;
  }
  // 判断 value 是否为 NaN
  return value === value ? value : 0;
}
```

## 原理

`toFinite` 实现比较简单，判断是否为 `falsey`，是则返回 `0`，接下来对参数做 `Number` 类型转换，判断值否为 无限大，是则将返回为同向有限数，否则返回对应数值, `NaN` 情况返回 `0`。

## 知识点

1. JavaScript 里所能表示的最大值 `Number.MAX_VALUE` 约等于 `1.7976931348623157e308`
2. `value === value` 用来判断等同于 `!isNaN(value)`

## 相关链接

- [lodash 源码阅读 —— toNumber](../Lang/toNumber.md)
- [MDN - Number.MAX_VALUE](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_VALUE)
