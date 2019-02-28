# lodash 源码分析 —— multiply

> Good ideas are always crazy until they’re not.  
> -Larry Page

## 依赖

```js
import createMathOperation from './.internal/createMathOperation.js';
```

[lodash 源码分析 —— createMathOperation](../Internal/createMathOperation.md)

## 源码

```js
/**
 * 两个数相乘
 *
 * @since 4.7.0
 * @category Math
 * @param {number} multiplier 相乘的第一个数。
 * @param {number} multiplicand 相乘的第二个数。
 * @returns {number} 返回乘积。
 * @example
 *
 * multiply(6, 4)
 * // => 24
 */
const multiply = createMathOperation(
  (multiplier, multiplicand) => multiplier * multiplicand,
  1
);
```

## 原理

`multiply` 实现原理很简单，使用 `createMathOperation` 高阶函数，封装一次乘法操作。`createMathOperation` 官方描述其作用是：`创建一个对两个值执行数学运算的函数`，我们可以理解成这个方法给传入的数学运算函数，加入了对传入参数进行边界检查及类型转换的功能，使其能正确返回用户想获取的值。具体实现可以查阅 [lodash 源码分析 —— createMathOperation](../Internal/createMathOperation.md)。

## 相关链接

- [lodash 技巧 —— 高阶函数](./Tips/higherOrderFunction.md)
