# lodash 源码阅读 —— baseFor

> "I'm not in this world to live up to your expectations and you're not in this world to live up to mine." —Bruce Lee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * `baseForOwn` 的基本实现，它迭代 `keysFunc` 返回的 `object` 属性，并为每个属性调用 `iteratee`。
 * Iteratee 函数可以通过显式返回 `false` 来提前退出迭代。
 *
 * @private
 * @param {Object} object 需要进行迭代的对象。
 * @param {Function} iteratee 每次迭代调用的函数。
 * @param {Function} keysFunc 获取 `object` 的 `keys` 信息的方法。
 * @returns {Object} 返回 `object`。
 */
function baseFor(object, iteratee, keysFunc) {
  const iterable = Object(object);
  // 获取属性值
  const props = keysFunc(object);
  let { length } = props;
  let index = -1;

  // 迭代
  while (length--) {
    const key = props[++index];
    if (iteratee(iterable[key], key, iterable) === false) {
      break;
    }
  }
  return object;
}
```

## 相关链接

- [lodash 源码阅读 —— baseFor](../Internal/baseFor.md)
- [lodash 源码阅读 —— keys](../Object/keys.md)
