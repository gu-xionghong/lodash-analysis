# lodash 源码阅读 —— random

> "Too many of us are not living our dreams because we are living our fears." —@LesBrown77

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import toFinite from './toFinite.js';
```

- [lodash 源码阅读 —— toFinite](../Lang/toFinite.md)

## 源码

```js
/**内置方法引用，不依赖于`root`。 */
const freeParseFloat = parseFloat;

/**
 * 产生一个包括 lower 与 upper 之间的数。 如果只提供一个参数返回一个0到提供数之间的数。
 * 如果 floating 设为 true，或者 lower 或 upper 是浮点数，结果返回浮点数。
 *
 * 注意: JavaScript 遵循 IEEE-754 标准处理无法预料的浮点数结果。
 *
 * @since 0.7.0
 * @category Number
 * @param {number} [lower=0] 下限。
 * @param {number} [upper=1] 上限。
 * @param {boolean} [floating] 指定是否返回浮点数。
 * @returns {number} 返回随机数。
 */
function random(lower, upper, floating) {
  // 适配可选参数变量
  if (floating === undefined) {
    if (typeof upper == 'boolean') {
      floating = upper;
      upper = undefined;
    } else if (typeof lower == 'boolean') {
      floating = lower;
      lower = undefined;
    }
  }
  if (lower === undefined && upper === undefined) {
    lower = 0;
    upper = 1;
  } else {
    // 兼容 Infinity、-Infinity 情况
    lower = toFinite(lower);
    if (upper === undefined) {
      upper = lower;
      lower = 0;
    } else {
      upper = toFinite(upper);
    }
  }
  // 转换负范围
  if (lower > upper) {
    const temp = lower;
    lower = upper;
    upper = temp;
  }
  // 判断是否返回浮点数结果
  if (floating || lower % 1 || upper % 1) {
    const rand = Math.random();
    const randLength = `${rand}`.length - 1;
    return Math.min(lower + (rand * (upper - lower + freeParseFloat(`1e-${randLength}`)), upper));
  }
  return lower + Math.floor(Math.random() * (upper - lower + 1));
}
```

## 原理

`random` 方法前段部分对可选参数做了赋值转换，并对 `Infinity`、 `-Infinity` 参数进行 [toFinite](../Lang/toFinite.md) 有限化。  
接下来是取范围 `[lower, upper]` 间的伪随机数，这部分逻辑有两个注意点：

1. 修正右区间取值

```js
//  ...
return Math.min(lower + (rand * (upper - lower + freeParseFloat(`1e-${randLength}`)), upper));
//  ...
return lower + Math.floor(Math.random() * (upper - lower + 1));
```

不论返回浮点数结果还是整型结果，`random` 均在计算中加了一个最小单位值 **freeParseFloat(`1e-${randLength}`)** 或 **1**，这么做是因为 Javascript 的 `Math.random()` 函数返回一个伪随机数在范围 `[0，1)`，若要确保伪随机数包含右区间 `upper`，则需要添加一个最小单位值进行数据修正。

2. 修正浮点数运算精度问题

```js
// ...
return Math.min(lower + (rand * (upper - lower + freeParseFloat(`1e-${randLength}`)), upper));
// ...
```

在返回浮点数结果时，调用了 `Math.min(randomValue, upper)` 在伪随机数 `randomValue` 跟 `upper` 间区最小值，这么做是因为 `randomValue` 是通过 **lower** 跟浮点数 **(rand \* (upper - lower + freeParseFloat(`1e-${randLength}`))** 得到的，我们知道，在 JS 中，浮点数运算存在精度问题，一个经典案例：

```js
0.1 + 0.2;
// 0.30000000000000004

0.1 + 0.2 === 0.3;
// false

0.1 + 0.2 > 0.3;
// true
```

因为精度问题，存在`randomValue` 值大于右区间 `upper` 的可能性，故此处需要使用 `Math.min()` 进行数据修正。

既然提到了 `0.1 + 0.2 == 3` 为 `false` 的情况，这里提供一个方法避免这样的情况发生，使用 JavaScript 提供的最小精度值：

```js
Math.abs(0.1 + 0.2 - 0.3) < Number.EPSILON;
```

最后推荐一篇文章 —— [抓住数据的小尾巴 - JS 浮点数陷阱及解法](https://zhuanlan.zhihu.com/p/30703042)，这篇文章对我们更深入了解 js 精度问题有很大帮助。

## 相关链接

- [Wiki - IEEE754](https://zh.wikipedia.org/wiki/IEEE_754)
- [MDN - Number.EPSILON](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/EPSILON)
- [lodash 源码阅读 —— toFinite](../Lang/toFinite.md)
- [抓住数据的小尾巴 - JS 浮点数陷阱及解法](https://zhuanlan.zhihu.com/p/30703042)

## 参考

- [MDN - Math.random()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/random)
