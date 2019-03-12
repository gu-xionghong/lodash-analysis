# lodash 源码阅读 —— toNumber

> "A surplus of effort could overcome a deficit of confidence." —Sonia Sotomayor

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * 检查 value 是否为 Object 的
 * [language type](http://www.ecma-international.org/ecma-262/7.0/#sec-ecmascript-language-types)
 * (例如： arrays, functions, objects, regexes,new Number(0), 以及 new String(''))
 *
 * @since 0.1.0
 * @category Lang
 * @param {*} value 要检查的值。
 * @returns {boolean} 如果 value 为一个对象，那么返回 true，否则返回 false。
 */
function isObject(value) {
  const type = typeof value;
  return value != null && (type == 'object' || type == 'function');
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
