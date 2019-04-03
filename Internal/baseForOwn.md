# lodash 源码阅读 —— baseForOwn

> "I'm not in this world to live up to your expectations and you're not in this world to live up to mine." —Bruce Lee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import baseFor from './baseFor.js';
import keys from '../keys.js';
```

- [lodash 源码阅读 —— baseFor](../Internal/baseFor.md)
- [lodash 源码阅读 —— keys](../Object/keys.md)

## 源码

```js
/**
 * `forOwn` 的基础实现
 *
 * @private
 * @param {Object} object 需要进行迭代的对象
 * @param {Function} iteratee 每次迭代调用的函数
 * @returns {Object} 返回 `object`
 */
function baseForOwn(object, iteratee) {
  // 判断是否存在，并调用 baseFor, keyFuns 使用 keys
  return object && baseFor(object, iteratee, keys);
}
```

## 相关链接

- [lodash 源码阅读 —— baseFor](../Internal/baseFor.md)
- [lodash 源码阅读 —— keys](../Object/keys.md)
