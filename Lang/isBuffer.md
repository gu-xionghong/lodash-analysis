# lodash 源码阅读 —— isBuffer

> "I'm not in this world to live up to your expectations and you're not in this world to live up to mine." —Bruce Lee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import root from './.internal/root.js'; // 当前运行环境
```

## 源码

```js
/** 检测自由变量 `exports` */
const freeExports = typeof exports == 'object' && exports !== null && !exports.nodeType && exports;

/** 检测自由变量 `module` */
const freeModule = freeExports && typeof module == 'object' && module !== null && !module.nodeType && module;

/** 检测 CommonJS 扩展 `module.exports` */
const moduleExports = freeModule && freeModule.exports === freeExports;

/** 内置值引用 */
const Buffer = moduleExports ? root.Buffer : undefined;

/* 引用那些与其他 `lodash` 方法同名的内置方法 */
const nativeIsBuffer = Buffer ? Buffer.isBuffer : undefined;

/**
 *检查 value 是否是个 buffer。
 *
 * @since 4.3.0
 * @category Lang
 * @param {*} value 要检查的值。
 * @returns {boolean} 如果 value 是一个buffer，那么返回 true，否则返回 false。
 */
// 使用当前环境 Buffer.isBuffer 检测 buffer 变量
const isBuffer = nativeIsBuffer || (() => false);
```

## 相关链接

- [How to check whether a script is running under node.js](https://stackoverflow.com/questions/4224606/how-to-check-whether-a-script-is-running-under-node-js)
