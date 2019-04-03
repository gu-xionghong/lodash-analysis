# lodash 源码阅读 —— arrayReduce

> "I'm not in this world to live up to your expectations and you're not in this world to live up to mine." —Bruce Lee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * 数组 `reduce` 的专用版本。
 *
 * @private
 * @param {Array} [array] 迭代的数组。
 * @param {Function} iteratee 每次迭代调用的函数。
 * @param {*} [accumulator] 初始值
 * @param {boolean} [initAccum] 是否使用`array`的第一个元素指定为初始值。
 * @returns {*} 返回累计值。
 */
function arrayReduce(array, iteratee, accumulator, initAccum) {
  let index = -1;
  const length = array == null ? 0 : array.length;

  // 若 initAccum 为真且数组长度有效，则将数组第一个元素指定为初始值
  if (initAccum && length) {
    accumulator = array[++index];
  }

  // 迭代
  while (++index < length) {
    accumulator = iteratee(accumulator, array[index], index, array);
  }
  return accumulator;
}
```
