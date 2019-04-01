# lodash 源码阅读 —— flip

> "Use only that which works, and take it from any place you can find it." —Bruce Lee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * 创建一个会缓存 func 结果的函数。 如果提供了 resolver ，就用 resolver 的返回值作为 key 缓存函数的结果。
 * 默认情况下用第一个参数作为缓存的 key。 func 在调用时 this 会绑定在缓存函数上。
 *
 * 注意: 缓存会暴露在缓存函数的 cache 实例。
 * 它是可以定制的，只要替换了 _.memoize.Cache 构造函数
 * 或实现了 [`Map`](http://ecma-international.org/ecma-262/7.0/#sec-properties-of-the-map-prototype-object) 的
 * delete, get, has, 和 set方法的对象。
 *
 * @since 0.1.0
 * @category Function
 * @param {Function} func 需要缓存化的函数。
 * @param {Function} [resolver] 这个函数的返回值作为缓存的 key。
 * @returns {Function} 返回缓存化后的函数。
 */
function memoize(func, resolver) {
  // 判断 func、resolver 类型是否合法
  if (typeof func != 'function' || (resolver != null && typeof resolver != 'function')) {
    throw new TypeError('Expected a function');
  }
  // 创建函数 memoized
  const memoized = function(...args) {
    // 获取缓存 key，默认为传入的第一个参数
    const key = resolver ? resolver.apply(this, args) : args[0];
    // 获取 cache 实例
    const cache = memoized.cache;

    // 检测缓存中是否已有对应 key 的 result，有则直接返回
    if (cache.has(key)) {
      return cache.get(key);
    }

    // 调用 func 取得对应返回值，set 进缓存 cache 内
    const result = func.apply(this, args);
    memoized.cache = cache.set(key, result) || cache;
    return result;
  };
  // 实例化 memoized.cache，默认 Map，可通过修改 _.memoize.Cache 改变对应构造函数
  memoized.cache = new (memoize.Cache || Map)();
  return memoized;
}

memoize.Cache = Map;
```

## 原理

通过 `Map` 数据结构缓存函数运行结果，若缓存 `cache` 中已存在对应 `key` 结果，则直接返回数据不再执行 `func`，需要大量执行相同数据操作的函数,使用 `memoize` 可以极大提高运行效率。`memoize` 中有两个巧妙的用法：

1. 暴露 `memoized.cache`，提供用户 `crud` 缓存数据的功能；

```js
const object = { a: 1, b: 2 };

const memoized = memoize(({ a, b }) => b);
memoized(object); // 执行 func，返回 2
memoized(object); // 不执行 func，返回 2
memoized.cache.delete(object); // 删除缓存值
memoized(object); // 执行 func，返回 2
```

2. 提供 `memoize.Cache`，提供用户自定义类 `Map` 构造函数的功能，让 `memoize` 具有更强的可扩展性。

```js
const mapMemoized = memoize((...args) => args);
Object.prototype.toString.call(mapMemoized.cache);
// '[object Map]'

memoize.Cache = WeakMap;
const weakMapMemoized = memoize((...args) => args);
Object.prototype.toString.call(weakMapMemoized.cache);
// '[object WeakMap]'
```

## 相关链接

- [lodash 技巧 —— 高阶函数](../Tips/higherOrderFunction.md)
- [MDN - WeakMap](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)
- [MDN - Objects 和 maps 的比较](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map#Objects_%E5%92%8C_maps_%E7%9A%84%E6%AF%94%E8%BE%83)
