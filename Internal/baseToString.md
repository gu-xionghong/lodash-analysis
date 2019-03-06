# lodash 源码阅读 —— baseToString

> Be yourself. Everyone else is already taken.  
> — Oscar Wilde

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import isSymbol from '../isSymbol.js';
```

- [lodash 源码阅读 —— isSymbol](../Lang/isSymbol.md)

## 源码

```js
/** 声明 无穷大 变量  */
const INFINITY = 1 / 0;

/** 用于将 symbols 转化为原始值或字符串 */
const symbolToString = Symbol.prototype.toString;

/**
 * 优化实现 'toString'
 *
 * @private
 * @param {*} value 需要处理的值
 * @returns {string} 返回一个字符串
 */
function baseToString(value) {
  // 若参数为 string 类型，则提前返回自身，避免某些环境下的性能损失
  if (typeof value == 'string') {
    return value;
  }
  if (Array.isArray(value)) {
    // 递归进行值转换（易受调用堆栈限制）
    return `${value.map(baseToString)}`;
  }
  if (isSymbol(value)) {
    return symbolToString ? symbolToString.call(value) : '';
  }
  const result = `${value}`;
  return result == '0' && 1 / value == -INFINITY ? '-0' : result;
}
```

## 原理

`baseToString` 相较于 `toString`，主要有三个优化点：

1. 递归转换数组类型
2. `Symbol` 类型 `String` 化
3. 特殊处理 `-''`、`-null`、`-'0'`、`-false`等多种情况，返回`-0` 值

1、3 两点都比较容易理解，第 2 个点，有一个比较精妙的写法，便是缓存了 `Symbol.prototype.toString` 给 `symbolToString` 变量，这里为什么要这么做而不直接使用 `Symbol` 类型中的 `toString` 呢？
刚开始我也没有明白这样做的写法，后来在看了对应的 `isSymbol.test.js` 测试文件后，得到了启发，部分代码是这样的:

```js
var symbol = Symbol('a');
// ...
assert.strictEqual(isSymbol(symbol), true);
assert.strictEqual(isSymbol(Object(symbol)), true);
// ...
```

`Object(symbol)` 是一个包装对象，包装对象存在被复写 [原始数据](https://developer.mozilla.org/zh-CN/docs/Glossary/Primitive) 方法的可能:

```js
const symbol = Object(Symbol());
symbol.toString();
// "Symbol()"

symbol.toString = () => console.log('我被复写了');
symbol.toString();
// "我被复写了"
```

而使用 `Symbol.prototype.toString` 可以避免这种情况的发生：

```js
Symbol.prototype.toString.call(symbol);
// "Symbol()"
```

针对 `Symbol` 的特殊处理原因，我另起一篇文章讲解 [从 Symbol 延伸理解 JS 包装对象](../Tips/wrapper.md)

## 相关链接

- [从 Symbol 延伸理解 JS 包装对象](../Tips/wrapper.md)
- [lodash 源码阅读 —— isSymbol](../Lang/isSymbol.md)

## 参考

- [重学前端 | JavaScript 类型：关于类型，有哪些你不知道的细节？ ](https://time.geekbang.org/column/article/78884)
