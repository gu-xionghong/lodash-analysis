# lodash 源码阅读 —— arrayLikeKeys

> "I'm not in this world to live up to your expectations and you're not in this world to live up to mine." —Bruce Lee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import isArguments from '../isArguments.js';
import isBuffer from '../isBuffer.js';
import isIndex from './isIndex.js';
import isTypedArray from '../isTypedArray.js';
```

- [lodash 源码阅读 —— isArguments](../Lang/isArguments.md)
- [lodash 源码阅读 —— isBuffer](../Lang/isBuffer.md)
- [lodash 源码阅读 —— isIndex](../Internal/isIndex.md)
- [lodash 源码阅读 —— isTypedArray](../Lang/isTypedArray.md)

## 源码

```js
const hasOwnProperty = Object.prototype.hasOwnProperty;

/**
 * 创建类数组 `value` 的可枚举属性名的数组。
 *
 * @private
 * @param {*} value 要查询的值。
 * @param {boolean} inherited 是否返回继承属性名。
 * @returns {Array} 返回属性名数组。
 */
function arrayLikeKeys(value, inherited) {
  // 判断是否为数组类型
  const isArr = Array.isArray(value);
  // 判断是否为非数组且为 arguments 类型
  const isArg = !isArr && isArguments(value);
  // 判断是否为非数组、非 arguments 且为 buffer 类型
  const isBuff = !isArr && !isArg && isBuffer(value);
  // 判断是否为非数组、非 arguments、非 buffer 且为 typed  类型
  const isType = !isArr && !isArg && !isBuff && isTypedArray(value);
  const skipIndexes = isArr || isArg || isBuff || isType;
  const length = value.length;

  // 创建 result 变量缓存类数组索引属性值
  const result = new Array(skipIndexes ? length : 0);
  let index = skipIndexes ? -1 : length;
  while (++index < length) {
    result[index] = `${index}`;
  }

  // 遍历 value 属性值，根据 inherited 判断是否返回继承属性名，并跳过重复添加索引属性值
  for (const key in value) {
    if (
      // 判断是否添加继承属性名。
      (inherited || hasOwnProperty.call(value, key)) &&
      !(
        skipIndexes &&
        // Safari 9 严格模式中 `arguments.length` 是可枚举属性
        (key == 'length' ||
          // 跳过索引属性值
          isIndex(key, length))
      )
    ) {
      result.push(key);
    }
  }
  return result;
}
```

## 相关链接

- [MDN - for...in](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in)
- [MDN - for...of 与 for...in 的区别](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of#for...of%E4%B8%8Efor...in%E7%9A%84%E5%8C%BA%E5%88%AB)
- [lodash 源码阅读 —— isArguments](../Lang/isArguments.md)
- [lodash 源码阅读 —— isBuffer](../Lang/isBuffer.md)
- [lodash 源码阅读 —— isIndex](../Internal/isIndex.md)
- [lodash 源码阅读 —— isTypedArray](../Lang/isTypedArray.md)
