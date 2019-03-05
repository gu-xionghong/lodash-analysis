# lodash 源码分析 —— getTag

> Do what you love, not what you think you're supposed to do.
> — Unknown

## 源码

```js
const toString = Object.prototype.toString;

/**
 * 获取 value 的 Symbol.toStringTag
 *
 * @private
 * @param {*} value 需要查询的参数
 * @returns {string} 返回对应的 toStringTag
 */
function getTag(value) {
  if (value == null) {
    return value === undefined ? '[object Undefined]' : '[object Null]';
  }
  return toString.call(value);
}
```

## 原理

通过 `Object.prototype.toString` 获取 `value` 的 `Symbol.toStringTag`值。
但是许多内置的 JavaScript 对象类型即便没有 toStringTag 属性，也能被 toString() 方法识别并返回特定的类型标签，比如：

```js
Object.prototype.toString.call('foo'); // "[object String]"
Object.prototype.toString.call([1, 2]); // "[object Array]"
Object.prototype.toString.call(3); // "[object Number]"
Object.prototype.toString.call(true); // "[object Boolean]"
Object.prototype.toString.call(undefined); // "[object Undefined]"
Object.prototype.toString.call(null); // "[object Null]"
// ... and more
```

这里我们可以看到其中 `undefined` 跟 `null` 也能成功获取 `tag`，并不需要像源码中那样做 hack，[原因](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)是：

> 从 JavaScript1.8.5 开始 toString()调用 null 返回[object Null]，undefined 返回[object Undefined]，如第 5 版的 ECMAScript 和随后的 Errata。

## 参考

- [MDN - Symbol.toStringTag](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/toStringTag)
- [MDN - Object.prototype.toString()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)
