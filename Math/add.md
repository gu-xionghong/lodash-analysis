# lodash 源码阅读 —— add

> Everything you want to be, you already are. You're simply on the path to discovering it.  
> —Alicia Keys

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import createMathOperation from './.internal/createMathOperation.js';
```

[lodash 源码阅读 —— createMathOperation](../Internal/createMathOperation.md)

## 源码

```js
/**
 * 两个数相加。
 *
 * @since 3.4.0
 * @category Math
 * @param {number} augend 被加数
 * @param {number} addend 加数
 * @returns {number} 返回两数之和
 * @example
 *
 * add(6, 4)
 * // => 10
 */
const add = createMathOperation((augend, addend) => augend + addend, 0);
```

## 原理

`add` 实现原理很简单，使用 `createMathOperation` 高阶函数，封装一次加法操作。`createMathOperation` 官方描述其作用是：`创建一个对两个值执行数学运算的函数`，我们可以理解成这个方法给传入的数学运算函数，加入了对传入参数进行边界检查及类型转换的功能，使其能正确返回用户想获取的值。具体实现可以查阅 [lodash 源码阅读 —— createMathOperation](../Internal/createMathOperation.md)。

## 相关链接

- [lodash 技巧 —— 高阶函数](../Tips/higherOrderFunction.md)
