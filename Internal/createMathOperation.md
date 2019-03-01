# lodash 源码分析 —— createMathOperation

> Optimism is the faith that leads to achievement. Nothing can be done without hope and confidence.
>
> — Helen Keller

## 依赖

```js
import baseToNumber from './baseToNumber.js';
import baseToString from './baseToString.js';
```

- [lodash 源码分析 —— baseToNumber](../Internal/baseToNumber.md)
- [lodash 源码分析 —— baseToString](../Internal/baseToString.md)

## 源码

```js
/**
 * 创建对两个值做数学运算的函数
 *
 * @private
 * @param {Function} operator 执行数学运算的函数
 * @param {number} [defaultValue] 默认返回值（未传值）
 * @returns {Function} 返回新的数学运算函数
 */
function createMathOperation(operator, defaultValue) {
  return (value, other) => {
    // 当两个数都未传递时
    if (value === undefined && other === undefined) {
      return defaultValue;
    }
    // 当第二个数未传递时
    if (value !== undefined && other === undefined) {
      return value;
    }
    // 当第一个数未传递时
    if (other !== undefined && value === undefined) {
      return other;
    }
    // 当传递的参数类型存在字符串时，将两个值都转换成字符串
    if (typeof value === 'string' || typeof other === 'string') {
      value = baseToString(value);
      other = baseToString(other);
    }
    // 否则将两个值转成数字
    else {
      value = baseToNumber(value);
      other = baseToNumber(other);
    }
    return operator(value, other);
  };
}
```

## 原理

### 思路

`createMathOperation` 是一个高阶函数，它接收 `operator` 函数参数，在内部封装了一个对接收参数做类型检测并转换的新函数，并将其返回。

### undefined 检测

前三个 `if` 条件，对值为 `undefined` 做了检测，并返回对应值。  
这部分的判断条件为 `value === undefined` 而没有使用 `value === void 0`，这是为什么呢？难道 lodash 作者不担心 `undefined` 变量被全局修改吗？相关原因可以查看 [lodash 源码分析 —— isUndefined](./Lang/isUndefined.md)

### 类型转换

在接下来的处理中，新函数判断传递的参数中是否存在字符串，存在时则将两个变量进行 `baseToString` 转换进行数学运算，否则进行 `baseToNumber` 转换，这样做是为什么呢？
在 《Javascript 高级程序设计（第三版）》 第三章中介绍 `加性操作符-加法` 时，说明了这段规则：

> 如果有一个操作数是字符串，那么就要应用如下规则:
>
> - 如果两个操作数都是字符串，则将第二个操作数与第一个操作数拼接起来;
> - 如果只有一个操作数是字符串，则将另一个操作数转换为字符串，然后再将两个字符串拼接起来。

若存在一个操作数是字符串类型，javascript 会将两个操作数转化为 `String` 后进行操作，而 [baseToString](../Internal/baseToString.md) 则是对原生 `toString` 的优化。而在非加法的运算操作中，程序会将 非数值类型 进行 `Number` 化，所以提前将操作数 `String` 化，不会影响最终的运行结果。同理，[baseToNumber](../Internal/baseToNumber.md) 则是对 `Number()` 方法的优化。

## 相关链接

- [lodash 技巧 —— 高阶函数](./Tips/higherOrderFunction.md)
- [lodash 源码分析 —— isUndefined](./Lang/isUndefined.md)
- [lodash 源码分析 —— baseToNumber](../Internal/baseToNumber.md)
- [lodash 源码分析 —— baseToString](../Internal/baseToString.md)

## 参考

- 《Javascript 高级程序设计（第三版）》
