# lodash 源码阅读 —— round

> "A surplus of effort could overcome a deficit of confidence." —Sonia Sotomayor

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import createRound from './.internal/createRound.js';
```

- [lodash 源码阅读 —— createRound](../Internal/createRound.md)

## 源码

```js
/**
 * 根据 precision（精度） 四舍五入 number。
 *
 * @since 3.10.0
 * @category Math
 * @param {number} number 要四舍五入的数字。
 * @param {number} [precision=0] 四舍五入的精度。
 * @returns {number} 返回四舍五入的数字。
 * @example
 *
 * round(4.006)
 * // => 4
 *
 * round(4.006, 2)
 * // => 4.01
 *
 * round(4060, -2)
 * // => 4100
 */
const round = createRound('round');
```

## 原理

通过高阶函数 `createRound` 封装一个 `round` 处理方法，内部实现查看 —— [lodash 源码阅读 —— createRound](../Internal/createRound.md)

## 相关链接

- [lodash 技巧 —— 高阶函数](../Tips/higherOrderFunction.md)
- [lodash 源码阅读 —— createRound](../Internal/createRound.md)
