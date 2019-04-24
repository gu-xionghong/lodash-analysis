# lodash 源码阅读 —— map

> "It is our choices that show what we truly are, far more than our abilities." —@@jk_rowling

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 源码

```js
/**
 * 创建一个数组， value（值） 是 iteratee（迭代函数）遍历 collection（集合）中的每个元素后返回的结果。
 *  iteratee（迭代函数）调用3个参数： (value, index|key, collection).
 *
 * @since 5.0.0
 * @category Array
 * @param {Array} array 用来迭代的集合。
 * @param {Function} iteratee 每次迭代调用的函数。
 * @returns {Array} 返回新的映射后数组。
 */
function map(array, iteratee) {
  let index = -1;
  const length = array == null ? 0 : array.length;
  const result = new Array(length);

  while (++index < length) {
    result[index] = iteratee(array[index], index, array);
  }
  return result;
}
```

## 知识点

你可能会有点奇怪，原生的 `map` 方法基本没有兼容性的问题，为什么 lodash 还要实现一个 `map` 方法呢？原因是在于 `lodash` 中处理数组，会将数组当成 密集数组 对待，原生的 `map` 方法则将数组当成 稀疏数组 对待。

## 相关链接

- [JavaScript —— 稀疏数组与密集数组](#/Tips/denseAndSparseArrays.md)(TODO)
- [读 lodash 源码之从 slice 看稀疏数组与密集数组](https://segmentfault.com/a/1190000012074005)
- [ why not the 'baseslice' func use Array.slice(), loop faster than slice?](https://github.com/lodash/lodash/issues/2850)
