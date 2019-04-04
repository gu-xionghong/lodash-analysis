# lodash 源码阅读 —— countBy

> “Yesterday, you said tomorrow.” -Nike

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import baseAssignValue from './.internal/baseAssignValue.js';
import reduce from './reduce.js';
```

- [lodash 源码阅读 —— baseAssignValue](../Internal/baseAssignValue.md)
- [lodash 源码阅读 —— reduce](../Collection/reduce.md)

## 源码

```js
const hasOwnProperty = Object.prototype.hasOwnProperty;

/**
 * 创建一个组成对象，key（键）是经过 iteratee（迭代函数） 执行处理 collection 中每个元素后返回的结果，
 * 每个key（键）对应的值是 iteratee（迭代函数）返回该 key（键）的次数。
 * iteratee 调用一个参数：(value)。
 *
 * @since 0.5.0
 * @category Collection
 * @param {Array|Object} collection 一个用来迭代的集合。
 * @param {Function} iteratee 一个迭代函数，用来转换key（键）。
 * @returns {Object} 返回一个组成集合对象。
 */
function countBy(collection, iteratee) {
  // 使用 reduce 遍历 collection，根据 iteratee 计算 value 后得出的 key，记录其次数
  return reduce(
    collection,
    (result, value, key) => {
      key = iteratee(value);
      if (hasOwnProperty.call(result, key)) {
        ++result[key];
      } else {
        baseAssignValue(result, key, 1);
      }
      return result;
    },
    {}
  );
}
```

## 相关链接

- [lodash 源码阅读 —— baseAssignValue](../Internal/baseAssignValue.md)
- [lodash 源码阅读 —— reduce](../Collection/reduce.md)
