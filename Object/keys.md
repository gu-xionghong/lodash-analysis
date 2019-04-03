# lodash 源码阅读 —— keys

> "I'm not in this world to live up to your expectations and you're not in this world to live up to mine." —Bruce Lee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import arrayLikeKeys from './.internal/arrayLikeKeys.js';
import isArrayLike from './isArrayLike.js';
```

- [lodash 源码阅读 —— arrayLikeKeys](../Internal/arrayLikeKeys.md)
- [lodash 源码阅读 —— isArrayLike](../Lang/isArrayLike.md)

## 源码

```js
/**
 * 创建一个 object 的自身可枚举属性名为数组。
 * Note: 非对象的值会被强制转换为对象，查看 [ES spec](http://ecma-international.org/ecma-262/7.0/#sec-object.keys) 了解详情。
 *
 * @since 0.1.0
 * @category Object
 * @param {Object} object 要检索的对象。
 * @returns {Array} 返回包含属性名的数组。
 */
function keys(object) {
  // 判断是否为类数组参数，是则调用 arrayLikeKeys，否则调用 Object.keys 获取
  return isArrayLike(object) ? arrayLikeKeys(object) : Object.keys(Object(object));
}
```

## 相关链接

- [ES spec - Object.keys](http://ecma-international.org/ecma-262/9.0/#sec-object.keys)
- [lodash 源码阅读 —— arrayLikeKeys](../Internal/arrayLikeKeys.md)
- [lodash 源码阅读 —— isArrayLike](../Lang/isArrayLike.md)
