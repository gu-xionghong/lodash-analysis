# lodash 源码阅读 —— baseReduce

> "I'm not in this world to live up to your expectations and you're not in this world to live up to mine." —Bruce Lee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * `reduce`和`reduceRight`的基本实现。
 * 使用 `eachFunc` 迭代 `collection`。
 *
 * @private
 * @param {Array|Object} collection 迭代的集合。
 * @param {Function} iteratee 每次迭代调用的函数。
 * @param {*} accumulator 初始值。
 * @param {boolean} initAccum 指定使用 `collection` 的第一个或最后一个元素作为初始值。
 * @param {Function} eachFunc 迭代`collection`的函数。
 * @returns {*} 返回累计值。
 */
function baseReduce(collection, iteratee, accumulator, initAccum, eachFunc) {
  eachFunc(collection, (value, index, collection) => {
    // 若 initAccum 为真，则将 collection 第一个元素指定为初始值
    accumulator = initAccum ? ((initAccum = false), value) : iteratee(accumulator, value, index, collection);
  });
  return accumulator;
}
```
