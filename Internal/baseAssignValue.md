# lodash 源码阅读 —— baseAssignValue

> “Yesterday, you said tomorrow.” -Nike

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * `assignValue` 和 `assignMergeValue` 无值检查的基本实现。
 *
 * @private
 * @param {Object} object 需要修改的对象。
 * @param {string} key 指定的 key
 * @param {*} value 需要分配的 value
 */
function baseAssignValue(object, key, value) {
  if (key == '__proto__') {
    Object.defineProperty(object, key, {
      configurable: true,
      enumerable: true,
      value: value,
      writable: true
    });
  } else {
    object[key] = value;
  }
}
```

## 知识点

`baseAssignValue` 是在一个对象上定义一个新属性，功能类似于 [Object.defineProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)（笔者认为可以直接用该方法替代），方法内部对 `key` 做了是否等于 `__proto__`
的判断，这样做是因为对于一个对象来说，`__proto__` 属于 `Object.prototype`，直接赋值 `object.__proto__` 修改的是其原型链上的属性 `__proto__`，而非自身的属性，需要给 `object` 添加自身属性 `__proto__`，需要使用到 [Object.defineProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)。

## 相关链接

- [Object.defineProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)
- [Object.prototype.\_\_proto\_\_](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/proto)
- [Why does Object.getOwnPropertyDescriptor({}, '\_\_proto\_\_') return undefined?](https://stackoverflow.com/questions/36116964/why-does-object-getownpropertydescriptor-proto-return-undefined)
