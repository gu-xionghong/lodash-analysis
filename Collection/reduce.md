# lodash 源码阅读 —— reduce

> "I'm not in this world to live up to your expectations and you're not in this world to live up to mine." —Bruce Lee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import arrayReduce from './.internal/arrayReduce.js';
import baseEach from './.internal/baseEach.js';
import baseReduce from './.internal/baseReduce.js';
```

- [lodash 源码阅读 —— arrayReduce](../Internal/arrayReduce.md)
- [lodash 源码阅读 —— baseEach](../Internal/baseEach.md)
- [lodash 源码阅读 —— baseReduce](../Internal/baseReduce.md)

## 源码

```js
/**
 * 压缩 collection（集合）为一个值，通过 iteratee（迭代函数）遍历 collection（集合）中的每个元素，每次返回的值会作为下一次迭代使用。
 * 如果没有提供 accumulator，则 collection（集合）中的第一个元素作为初始值。
 * iteratee 调用4个参数：(accumulator, value, index|key, collection).
 *
 * @since 0.1.0
 * @category Collection
 * @param {Array|Object} collection 用来迭代的集合。
 * @param {Function} iteratee 每次迭代调用的函数。
 * @param {*} [accumulator] 初始值。
 * @returns {*} 返回累加后的值。
 */
function reduce(collection, iteratee, accumulator) {
  const func = Array.isArray(collection) ? arrayReduce : baseReduce;
  const initAccum = arguments.length < 3;
  // 根据 collection 类型选择调用 arrayReduce 或 baseReduce
  return func(collection, iteratee, accumulator, initAccum, baseEach);
}
```

## 相关链接

- [MDN - Arguments](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arguments)
- [lodash 源码阅读 —— arrayReduce](../Lang/arrayReduce.md)
- [lodash 源码阅读 —— baseEach](../Lang/baseEach.md)
- [lodash 源码阅读 —— baseReduce](../Lang/baseReduce.md)
