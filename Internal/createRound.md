# lodash 源码阅读 —— ceil

> "A surplus of effort could overcome a deficit of confidence." —Sonia Sotomayor

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * 创建一个像 round 类似函数。
 *
 * @private
 * @param {string} methodName 使用 Math中对应的 rounding 方法 ceil/floor/round。
 * @returns {Function} 返回新的 round 函数。
 */
function createRound(methodName) {
  const func = Math[methodName];
  return (number, precision) => {
    precision = precision == null ? 0 : precision >= 0 ? Math.min(precision, 292) : Math.max(precision, -292);
    if (precision) {
      // 使用科学计数法移位以避免浮点问题。
      // 查看 [MDN](https://mdn.io/round#Examples) 获取更多详情。
      let pair = `${number}e`.split('e');
      const value = func(`${pair[0]}e${+pair[1] + precision}`);

      pair = `${value}e`.split('e');
      return +`${pair[0]}e${+pair[1] - precision}`;
    }
    return func(number);
  };
}
```

## 原理

`createRound` 是一个高阶函数，它先将数值转化为 `科学计数法` 表示的字符串，再通过 `移位` 形式，对数值进行 `ceil/floor/round` 操作，最后恢复对应 位值 返回。

### 极限精度值 1e292/1e-292 (TODO)

关于精度 `292/-292` 问题，作者研究了一天也没有理清楚，相关 [issue](https://github.com/lodash/lodash/issues/4232)

### 科学计数法移位计算

`科学计数法移位` 计算的方式可以避免浮点数精度问题，方法逻辑可以剖析成下面流程：

```js
// _.ceil(6.004, 2)
const precision = 2;
const number = 6.004;

// 科学计数法
const pair = 0; // 对应指数
const expNumber = '6.004e0';

// 移位 2 个精度
const shiftedExpNumber = `6.004e${0 + precision}`; // 6.004e2
const shiftedNumber = 600.4;

// 进行 ceil 转换
const ceiledShiftedNumber = Math.ceil(shiftedNumber); // 601

// 恢复对应位数
const ceiledNumber = +`${ceiledShiftedNumber}e{0 - precision}`; // +'601e-2' -> 6.01
```

## 相关链接

- [lodash 技巧 —— 高阶函数](../Tips/higherOrderFunction.md)
- [lodash 源码阅读 —— ceil](../Math/ceil.md)
- [lodash 源码阅读 —— floor](../Math/floor.md)
- [lodash 源码阅读 —— round](../Math/round.md)
