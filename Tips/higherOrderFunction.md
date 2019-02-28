# lodash 技巧 —— 高阶函数

> Never look back unless you are planning to go that way.  
> —Henry David Thoreau

## 定义

Wiki 中这样定义 (高阶函数（Higher-order function）)[https://zh.wikipedia.org/wiki/%E9%AB%98%E9%98%B6%E5%87%BD%E6%95%B0]：

> 高阶函数是至少满足下列一个条件的函数：
>
> - 接受一个或多个函数作为输入
> - 输出一个函数

`JavaScript` 语言中的函数显然满足高阶函数的条件，在实际开发中，无论是将函数当作参数传递，还是让函数的执行结果返回另外一个函数，这两种情形都有很多应用场景。

- 函数作为参数传入

```js
function add(a, b, fn) {
  return fn(a) + fn(b);
}
var fn = function(a) {
  return a * a;
};
var fn2 = function(a) {
  return a + a;
};
add(2, 3, fn); //13
add(2, 3, fn2); //10
```

- 函数作为返回值

```js
// 单例模式
var getSingle = function(fn) {
  var ret;
  return function() {
    return ret || (ret = fn.apply(this, arguments));
  };
};
```

## 作用

### 代码复用

(DRY (Don’t repeat yourself))[https://en.wikipedia.org/wiki/Don%27t_repeat_yourself] 是优秀代码遵循的规则之一，编写高阶函数进行函数封装，可以大量减少重复代码的出现。

### 提高执行效率

通过闭包方式，把高阶函数接收的差异化参数提前固化，在执行返回函数时，则可以免去固化时的性能消耗了。

## 案例

### [lodash createMathOperation](../Internal/createMathOperation.md)

`lodash` 内置的 `createMathOperation` 方法，它的作用是：`创建对两个值执行数学运算的函数`，它给传入函数封装了对传入参数进行边界检查及类型转换的功能，在 (`_.add`)[../Math/add.md]、(`_.divide`)[../Math/divide.md]、(`_.multiply`)[../Math/multiply.md]、(`_.subtract`)[../Math/subtract.md] 等几个数学运算的方法里，均使用高阶函数 `createMathOperation` 对基础运算做了一层封装，这样极大的减少了重复书写代码的情况。

### [React Higher-Order Components](https://reactjs.org/docs/higher-order-components.html)

> Concretely, a higher-order component is a function that takes a component and returns a new component.

`React` 的 `HOC` 也是高阶函数的一种应用，创建一个方法，接受 `component` (转化后实则也是一个 `function`)，并返回一个赋予了新能力的新组件。之前在使用 `Vue` 时，作者也探索了 `HOC` 在 `Vue` 实现的方式：[VueJs - 高阶组件实践](http://blog.amu.fun/2017/07/06/knowledge/vue/vuejs%E9%AB%98%E9%98%B6%E7%BB%84%E4%BB%B6%E5%AE%9E%E8%B7%B5/)

### [Vue createPatchFunction](https://github.com/vuejs/vue/blob/59d8579fbd3a3450ab684d6a596f1853f2257d08/src/platforms/web/runtime/patch.js#L12)

`Vue` 源码中，在创建 `patch` 更新方法时，使用了 `createPatchFunction` 高阶函数，其接受两个参数，包含了不同平台下的 `nodeOps` 和 `modules` 参数。其中，`nodeOps` 封装了一系列 `DOM` 操作的方法，`modules` 定义了一些模块的钩子函数的实现，方法内部定义了一系列的辅助方法，最终返回了一个 `patch` 方法。  
 在 `Vue` 中，不同平台的 `patch` 主要逻辑部分是相同的，差异化部分只需要通过参数 `nodeOps` 和 `modules` 来区别，这里便用到了一个高阶函数函数 [柯里化](https://zh.wikipedia.org/wiki/%E6%9F%AF%E9%87%8C%E5%8C%96) 的技巧，通过 `createPatchFunction` 把差异化参数提前固化，这样不用每次调用 `patch` 的时候都传递 `nodeOps` 和 `modules` 了。

## 相关链接

- [ES7 修饰器](http://blog.amu.fun/2017/05/29/knowledge/ES7%E4%BF%AE%E9%A5%B0%E5%99%A8/)
- [函数节流 - throttle](http://blog.amu.fun/2017/06/07/knowledge/throttle/)

## 参考

- (Wiki —— 高阶函数)[https://zh.wikipedia.org/wiki/%E9%AB%98%E9%98%B6%E5%87%BD%E6%95%B0]
- (Vue.js 技术揭秘 —— update)[https://ustbhuangyi.github.io/vue-analysis/data-driven/update.html#%E6%80%BB%E7%BB%93]
- 《Javascript 设计模式与开发实践》
