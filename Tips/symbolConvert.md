# 从 ECMAScript 中理解 Symbol 类型转换

> Be kind whenever possible. It is always possible. —Dalai Lama

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 前言

在阅读 [.inernal.createMathOperation](../Internal/createMathOperation.md) 时，发现 lodash 在实现数学运算的时候，均对 `Symbol` 类型值做了特殊转换处理，下面我们来一探究竟这样做的原因：

## 特殊性

`Symbol`在进行原生类型转换时，存在特殊性。[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol#Symbol_type_conversions) 介绍到使用 `symbol` 值进行类型转换时需要注意事项：

> 1. When trying to convert a symbol to a number, a TypeError will be thrown (e.g. +sym or sym | 0).
> 2. Symbol("foo") + "bar" throws a TypeError (can't convert symbol to string). This prevents you from silently creating a new string property name from a symbol, for example.
> 3. When using loose equality, Object(sym) == sym returns true.
> 4. The "safer" String(sym) conversion works like a call to Symbol.prototype.toString() with symbols, but note that new String(sym) will throw.

我们用代码的形式描述上述情况：

```js
const symbol = Symbol('我是一个symbol');

+symbol;
// TypeError: Cannot convert a Symbol value to a number

symbol + 'bar';
// TypeError: Cannot convert a Symbol value to a string

Object(symbol) == symbol;
// true

String(symbol);
// "Symbol(我是一个symbol)"

new String(symbol);
// TypeError: Cannot convert a Symbol value to a string
```

接下来我们将从 [ecma-262](http://www.ecma-international.org/ecma-262/9.0/index.html#) 标准中理解这些情况发生的原因

## 隐式转换（implicit conversion）

在 JavaScript 中，当我们进行比较操作或者加减乘除四则运算操作时，常常会触发 JavaScript 的隐式类型转换机制。其中上述的 1、2 两点便是隐式转换带来的影响，我们先来看一下标准中 [加法运算](http://www.ecma-international.org/ecma-262/9.0/index.html#sec-addition-operator-plus) 的规则：

> Runtime Semantics: Evaluation#  
> AdditiveExpression:AdditiveExpression+MultiplicativeExpression
>
> 1. Let lref be the result of evaluating AdditiveExpression.
> 2. Let lval be ? `GetValue`(lref). // 通过内置 [GetValue](http://www.ecma-international.org/ecma-262/9.0/index.html#sec-getvalue) 方法获取参数，Symbol 类型直接返回自身
> 3. Let rref be the result of evaluating MultiplicativeExpression.
> 4. Let rval be ? `GetValue`(rref).
> 5. Let lprim be ? `ToPrimitive`(lval). // 通过内置 [ToPrimitive](http://www.ecma-international.org/ecma-262/9.0/index.html#sec-toprimitive) 方法获取参数原始值，Symbol 类型直接返回自身
> 6. Let rprim be ? `ToPrimitive`(rval).
> 7. If `Type`(lprim) is String or `Type`(rprim) is String, then
>    a. Let lstr be ? `ToString`(lprim).
>    b. Let rstr be ? `ToString`(rprim).
>    c. Return the String that is the result of concatenating lstr and rstr.
> 8. Let lnum be ? `ToNumber`(lprim).
> 9. Let rnum be ? `ToNumber`(rprim).
> 10. Return the result of applying the addition operation to lnum and rnum. See the Note below 12.8.5.

顺着这份规则顺序，当程序执行字符串加法 `symbol + 'bar'` 时，程序会对参数执行多次内置方法进行隐式转化，最终我们会进入第 7 点的分叉，这是很关键的一步，程序会对两个参数执行内置 `ToString` 方法进转换，接下来我们来认识下 `ToString` 方法。

## ToString 转换

[ToString](http://www.ecma-international.org/ecma-262/9.0/index.html#sec-tostring) 转换规则如下：
|参数类型 | 返回结果 |
| ------------- |:-------------:|
| Undefined | 返回 "undefined" |
| Null | 返回 "Null" |
| Boolean | 如果参数为真，返回 "true",否则返回 "false" |
| Number | 详见 [ToString Applied to the Number Type](http://www.ecma-international.org/ecma-262/9.0/index.html#sec-tostring-applied-to-the-number-type) |
| String | 返回参数自身 |
| Symbol | 抛出 `TypeError` 异常 |
| Object | 执行下面步骤： 1. 将参数以字符串优先形式执行 [ToPrimitive](http://www.ecma-international.org/ecma-262/9.0/index.html#sec-toprimitive). 2. 将原始值化的值再进行 ToString 运算 |

我们可以看到，只要是 `Symbol` 类型值进行 `ToString` 转换，程序直接抛出 `TypeError` 异常，[ToNumber](http://www.ecma-international.org/ecma-262/9.0/index.html#sec-tonumber) 方法也是如此，这样就讲得通为什么直接将 `Symbol` 值进行四则运算时，系统会抛出 `TypeError: Cannot convert a Symbol value to a TYPE` 的错误了。
为什么规范制定者们要加这条规则呢？

从 [MDN](#te-shu-xing) 里那句 `This prevents you from silently creating a new string property name from a symbol, for example.` 可以理解到，这其中一个原因是为了避免开发者在无意中使用 Symbol 类型值隐式的创建了一个字符串类型属性名，可能出现的代码情况如下：

```js
const symbol = Symbol();
const obj = {};

obj[symbol + ''] = '我期望是 Symbol 类型属性名对应值，但我其实是字符串';

obj[symbol] = '我是真正的 Symbol 类型属性名对应值';

console.log(obj);
// 如果 symbol + '' 不报错
// {
//   Symbol(): "我期望是 Symbol 类型属性名对应值，但我其实是字符串"
//   Symbol(): "我是真正的 Symbol 类型属性名对应值"
// }
```

## ToPrimitive 转换

[特殊性](#te-shu-xing) 中描述的第 3 点

> When using loose equality, Object(sym) == sym returns true.

当包装对象 `Object(sym)` 与原始值 `sym` 进行宽松对比时，返回值为 `true`。 [ecma262 - Abstract Equality Comparison](http://www.ecma-international.org/ecma-262/9.0/index.html#sec-abstract-equality-comparison) 中这么规定这种对比方式:

> If Type(x) is Object and Type(y) is either String, Number, or Symbol, return the result of the comparison ToPrimitive(x) == y.

当 对象类型 与 基本类型（`String/Number/Symbol`）进行宽松对比时，会将 对象类型值 传入 [ToPrimitive](http://www.ecma-international.org/ecma-262/9.0/index.html#sec-toprimitive) 方法进行 `拆箱转换` 成基本类型值，后再进行一次宽松对比，而 `Symbol` 包装对象 `Object(sym)` 拆箱后得到的便是 `sym`，即:

```js
const symbol = Symbol();
const symbolObject = Object(symbol);

ToPrimitive(symbolObject) === symbol; // true

// so
Object(symbolObject) == symbol; // true
```

## String() 与 new String() 差异

在 [ecma262 - String(value)](http://www.ecma-international.org/ecma-262/9.0/index.html#sec-string-constructor-string-value) 是这么定义 `String(value)` 的：

> When String is called with argument value, the following steps are taken:
>
> 1. If no arguments were passed to this function invocation, let s be "".
> 2. Else,
>    a. If `NewTarget` is undefined and Type(value) is Symbol, return [SymbolDescriptiveString](http://www.ecma-international.org/ecma-262/9.0/index.html#sec-symboldescriptivestring)(value).
>    b. Let s be ? ToString(value).
> 3. If `NewTarget` is undefined, return s.
> 4. Return ? StringCreate(s, ? GetPrototypeFromConstructor(`NewTarget`, "%StringPrototype%")).

其中 [NewTarget](http://www.ecma-international.org/ecma-262/9.0/index.html#_ref_157) 是在使用 `new` 操作符实例化对象后产生的值（即调用了构造器生成对象），即执行 `String()` 时，程序会进入 `2-a` 流程，而 `new String()` 则会进入 `2-b` 流程

```js
const symbol = Symbol();

String(symbol);
// 等同于
SymbolDescriptiveString(symbol);

new String(symbol);
// 等同于
ToString(symbol);
// or and more...
```

我们可以看出，[SymbolDescriptiveString](http://www.ecma-international.org/ecma-262/9.0/index.html#sec-symboldescriptivestring) 便是 [Symbol.prototype.toString()](http://www.ecma-international.org/ecma-262/9.0/index.html#sec-symbol.prototype.tostring) 最终执行的方法，所以 `String(symbol)` 最终能成功转化为对应的字符串值，而 `new String(symbol)` 在执行过程中，调用了 `ToString` 进行隐式转换，在 [ToString 转换](#tostring-zhuan-huan) 中我们已经了解到此时程序会抛出 `TypeError` 异常，流程也就不会继续往下走了。

## 结语

到这里，我们便把 `Symbol` 类型转换的特殊部分，从语言标准规范中理解了一遍。如果这篇文章有帮助到你，给个 star✨ 呗 🍭~

## 相关链接

- [MDN - Symbol.toPrimitive](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/toPrimitive)
- [MDN - Symbol.prototype.toString()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/toString)
- [MDN - Symbol.prototype.valueOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/valueOf)

## 参考

- [ecma-262](http://www.ecma-international.org/ecma-262/9.0/index.html#)
- [重学前端 | JavaScript 类型：关于类型，有哪些你不知道的细节？ ](https://time.geekbang.org/column/article/78884)
- [stackoverflow - why cannot convert a Symbol value to a string](https://stackoverflow.com/questions/44425974/why-cannot-convert-a-symbol-value-to-a-string)
