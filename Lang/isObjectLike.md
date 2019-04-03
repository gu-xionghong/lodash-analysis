# lodash 源码阅读 —— isObjectLike

> "This is your life. Do what you love, and do it often." —@Holstee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * 检查 value 是否是 类对象。
 * 如果一个值是类对象，那么它不应该是 null，而且 typeof 后的结果是 "object"。
 *
 * @since 4.0.0
 * @category Lang
 * @param {*} value 要检查的值。
 * @returns {boolean}  如果 value 为一个类对象，那么返回 true，否则返回 false。
 * @example
 *
 * isObjectLike({})
 * // => true
 *
 * isObjectLike([1, 2, 3])
 * // => true
 *
 * isObjectLike(Function)
 * // => false
 *
 * isObjectLike(null)
 * // => false
 */
function isObjectLike(value) {
  return typeof value == 'object' && value !== null;
}
```

## 知识点

### null

```js
typeof null === 'object'; // 从一开始出现JavaScript就是这样的
```

### typeof

| val 类型                                | 结果                                                               |
| --------------------------------------- | ------------------------------------------------------------------ |
| Undefined                               | "undefined"                                                        |
| Null                                    | "object"                                                           |
| Boolean                                 | "boolean"                                                          |
| Number                                  | "number"                                                           |
| String                                  | "string"                                                           |
| Symbol                                  | "symbol"                                                           |
| Object（原生，且没有实现 [[call]]）     | "object"                                                           |
| Object（标准宿主，且没有实现 [[call]]） | "object"                                                           |
| Object（原生或者宿主且实现了 [[call]]） | "function"                                                         |
| Object（非标准宿主且没实现 [[call]]）   | 由实现定义，但不能是 "undefined", "boolean", "number", or "string" |

## 相关链接

- [MDN - typeof](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/typeof)
- [The history of "typeof null"](http://2ality.com/2013/10/typeof-null.html)
