# lodash 源码分析 —— baseToNumber

> Overthinking ruins you. Ruins the situation, twists it around, makes you worry and just makes everything much worse than it actually is.
> — Unknown

## 依赖

```js
import isSymbol from '../isSymbol.js';
```

- [lodash 源码分析 —— isSymbol](../Lang/isSymbol.md)

## 源码

```js
/** 声明 NAN 变量 */
const NAN = 0 / 0;

/**
 * `toNumber`的基本实现，它不能确保正确转换二进制，十六进制或八进制字符串值。
 *
 * @private
 * @param {*} value 需要处理的值
 * @returns {number} 返回一个数值
 */
function baseToNumber(value) {
  if (typeof value == 'number') {
    return value;
  }
  if (isSymbol(value)) {
    return NAN;
  }
  return +value;
}
```

## 原理

`baseToNumber` 相较于 `Number()`，主要增加了对 `Symbol` 转换的优化，针对 `Symbol` 的特殊处理原因，我另起一篇文章讲解 [从 Symbol 延伸理解 JS 包装对象](../Tips/wrapper.md)。

## 相关链接

- [从 Symbol 延伸理解 JS 包装对象](../Tips/wrapper.md)
- [lodash 源码分析 —— isSymbol](../Lang/isSymbol.md)