# lodash 源码阅读 —— partition

> “Yesterday, you said tomorrow.” -Nike

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import reduce from './reduce.js';
```

- [lodash 源码阅读 —— reduce](../Collection/reduce.md)

## 源码

```js
/**
 * 创建一个分成两组的元素数组
 * 第一组包含predicate（断言函数）返回为 truthy（真值）的元素
 * 第二组包含predicate（断言函数）返回为 falsey（假值）的元素。
 * predicate 调用1个参数：(value)。
 *
 * @since 3.0.0
 * @category Collection
 * @param {Array|Object} collection 用来迭代的集合。
 * @param {Function} predicate 每次迭代调用的函数。
 * @returns {Array} 返回元素分组后的数组。
 * @example
 *
 * const users = [
 *   { 'user': 'barney',  'age': 36, 'active': false },
 *   { 'user': 'fred',    'age': 40, 'active': true },
 *   { 'user': 'pebbles', 'age': 1,  'active': false }
 * ]
 *
 * partition(users, ({ active }) => active)
 * // => objects for [['fred'], ['barney', 'pebbles']]
 */
function partition(collection, predicate) {
  // 使用 reduce，累计一个 二维数组 [[],[]]，根据 predicate（断言函数） 的情况归类集合内value
  return reduce(collection, (result, value, key) => (result[predicate(value) ? 0 : 1].push(value), result), [[], []]);
}
```

## 相关链接

- [lodash 源码阅读 —— reduce](../Collection/reduce.md)
