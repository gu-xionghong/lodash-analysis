# lodash 源码阅读 —— baseEach

> "I'm not in this world to live up to your expectations and you're not in this world to live up to mine." —Bruce Lee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import baseForOwn from './baseForOwn.js';
import isArrayLike from '../isArrayLike.js';
```

- [lodash 源码阅读 —— baseForOwn](../Internal/baseForOwn.md)
- [lodash 源码阅读 —— isArrayLike](../Lang/isArrayLike.md)

## 源码

```js
/**
 * forEach 的基础实现
 *
 * @private
 * @param {Array|Object} collection 需要迭代操作的集合
 * @param {Function} iteratee 每次迭代调用的函数
 * @returns {Array|Object} 返回新合集 `collection`.
 */
function baseEach(collection, iteratee) {
  if (collection == null) {
    return collection;
  }
  // 判断是否为类数组参数，否则返回执行 baseForOwn
  if (!isArrayLike(collection)) {
    return baseForOwn(collection, iteratee);
  }
  const length = collection.length;
  // 装箱 collection，兼容原始类型情况，例如 string
  const iterable = Object(collection);
  let index = -1;

  while (++index < length) {
    if (iteratee(iterable[index], index, iterable) === false) {
      break;
    }
  }
  return collection;
}
```

## 相关链接

- [lodash 源码阅读 —— baseForOwn](../Internal/baseForOwn.md)
- [lodash 源码阅读 —— isArrayLike](../Lang/isArrayLike.md)
