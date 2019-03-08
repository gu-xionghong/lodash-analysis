# lodash 源码阅读 —— toNumber

> "Too many of us are not living our dreams because we are living our fears." —@LesBrown77

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import isObject from './isObject.js';
import isSymbol from './isSymbol.js';
```

- [lodash 源码阅读 —— isObject](#/Lang/isObject.md)
- [lodash 源码阅读 —— isSymbol](../Lang/isSymbol.md)

## 源码

```js
const NAN = 0 / 0;
const freeParseInt = parseInt;

/** 用于匹配前后空格 */
const reTrim = /^\s+|\s+$/g;

/** 用于检测错误的带符号十六进制字符串值。 */
const reIsBadHex = /^[-+]0x[0-9a-f]+$/i;

/** 用于检测二进制字符串值。 */
const reIsBinary = /^0b[01]+$/i;

/** 用于检测八进制字符串值。 */
const reIsOctal = /^0o[0-7]+$/i;

/**
 * 将 value 转换为一个数字。
 *
 * @since 4.0.0
 * @category Lang
 * @param {*} value 要处理的值。
 * @returns {number} 返回数字。
 */
function toNumber(value) {
  // Number 类型，直接返回
  if (typeof value == 'number') {
    return value;
  }
  // Symbol 类型，返回 NaN
  if (isSymbol(value)) {
    return NAN;
  }
  // Object类型，模拟进行 PreferredType 为 number 的 ToPrimitive 拆箱操作
  if (isObject(value)) {
    const other = typeof value.valueOf == 'function' ? value.valueOf() : value;
    value = isObject(other) ? `${other}` : other;
  }
  // 拆箱完成，`ToString` 化结果若非 String，则将其 Number 化返回
  if (typeof value != 'string') {
    return value === 0 ? value : +value;
  }
  // 去除首尾空格
  value = value.replace(reTrim, '');
  // 是否为二进制
  const isBinary = reIsBinary.test(value);
  // 进行二进制、八进制判断，并将其转换为对应十进制数值
  return isBinary || reIsOctal.test(value)
    ? freeParseInt(value.slice(2), isBinary ? 2 : 8)
    : // 十六进制返回 NaN
    reIsBadHex.test(value)
    ? NAN
    : +value;
}
```

## 原理

## 知识点

## 相关链接

- [lodash 源码阅读 —— isObject](#/Lang/isObject.md)
- [lodash 源码阅读 —— isSymbol](../Lang/isSymbol.md)
