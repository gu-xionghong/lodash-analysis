# lodash 源码分析 —— isSymbol

> Each day wake up and ask yourself what will make you feel most alive that day.
> — Unknown

## 依赖

```js
import getTag from './.internal/getTag.js';
```

- [lodash 源码分析 —— getTag](../Internal/getTag.md)

## 源码

```js
/**
 * 检查 value 是否是 Symbol 原始值或者对象。
 *
 * @since 4.0.0
 * @category Lang
 * @param {*} value 要检查的值。
 * @returns {boolean} 如果 value 为一个symbol，那么返回 true，否则返回 false。
 * @example
 *
 * isSymbol(Symbol.iterator)
 * // => true
 *
 * isSymbol('abc')
 * // => false
 */
function isSymbol(value) {
  const type = typeof value;
  return type == 'symbol' || (type == 'object' && value != null && getTag(value) == '[object Symbol]');
}
```

## 原理

`Symbol` 原始值可以直接使用 `typeof` 进行类型检查:

```js
const symbol = Symbol();
typeof symbol;
// symbol
```

而包装对象下的 `Symbol`，则需要第二段逻辑进行判断：

```js
const symbolObject = Object(Symbol());
typeof symbolObject;
// object

getTag(symbolObject);
// "[object Symbol]"
```

## 相关链接

- [lodash 源码分析 —— getTag](../Internal/getTag.md)
- [从 Symbol 延伸理解 JS 包装对象](../Tips/wrapper.md)
