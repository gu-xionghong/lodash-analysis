# lodash 源码分析 —— isUndefined

> Every person is a new door to a different world.
>
> — Six Degrees of Separation

## 源码

```js
/**
 * 检查 value 是否是 undefined.
 *
 * @since 0.1.0
 * @category Lang
 * @param {*} value 要检查的值。
 * @returns {boolean} 如果 value 是 undefined ，那么返回 true，否则返回 false。
 * @example
 *
 * isUndefined(void 0)
 * // => true
 *
 * isUndefined(null)
 * // => false
 */
function isUndefined(value) {
  return value === undefined;
}
```

## 原理

这个函数很简单，只有一个判断条件为 `value === undefined`，但这里没有使用 `value === void 0` 是为什么呢？难道 lodash 作者不担心 `undefined` 变量被全局修改吗？查阅资料后发现，[issue](https://github.com/lodash/lodash/issues/4041) 中也曾有人提出这样的疑惑，官方回应是：

> In ES5 browsers it's not a concern.

支持 `ES5` 的浏览器，将不再需要考虑这个问题。查阅 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) 了解到:

> 在现代浏览器（JavaScript 1.8.5/Firefox 4+），自 ECMAscript5 标准以来 undefined 是一个不能被配置（non-configurable），不能被重写（non-writable）的属性。即便事实并非如此，也要避免去重写它。

现今的浏览器大部分均已实现 `ES5` 标准，所以这个顾虑也不再考虑范围了。但是大家在局部作用域时，还是需要考虑 `undefined` 被重写的风险：

> 但是它有可能在非全局作用域中被当作标识符（变量名）来使用(因为 undefined 不是一个保留字))，这样做是一个非常坏的主意，因为这样会使你的代码难以去维护和排错。

## 参考

- [MDN - undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)
