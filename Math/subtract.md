# lodash 源码分析 —— subtract

> You are stronger than your challenges and your challenges are making you stronger.  
> -Karen Salmansohn

## 依赖

```js
import createMathOperation from './.internal/createMathOperation.js';
```

[lodash 源码分析 —— createMathOperation](../Internal/createMathOperation.md)

## 源码

```js
/**
 * 两数相减。
 *
 * @since 4.0.0
 * @category Math
 * @param {number} minuend 被减数
 * @param {number} subtrahend 减数
 * @returns {number} 返回差
 * @example
 *
 * subtract(6, 4)
 * // => 2
 */
const subtract = createMathOperation((minuend, subtrahend) => minuend - subtrahend, 0);
```

## 原理

`subtract` 实现原理很简单，使用 `createMathOperation` 高阶函数，封装一次减法操作。`createMathOperation` 官方描述其作用是：`创建一个对两个值执行数学运算的函数`，我们可以理解成这个方法给传入的数学运算函数，加入了对传入参数进行边界检查及类型转换的功能，使其能正确返回用户想获取的值。具体实现可以查阅 [lodash 源码分析 —— createMathOperation](../Internal/createMathOperation.md)。

## 相关链接

- [lodash 技巧 —— 高阶函数](../Tips/higherOrderFunction.md)
