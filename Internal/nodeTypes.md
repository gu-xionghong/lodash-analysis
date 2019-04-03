# lodash 源码阅读 —— nodeTypes

> "I'm not in this world to live up to your expectations and you're not in this world to live up to mine." —Bruce Lee

本文为 《lodash 源码阅读》 系列文章，后续内容会在 [github](https://github.com/gu-xionghong/lodash-analysis) 中发布，欢迎 star，[gitbook](https://gu-xionghong.gitbook.io/lodash-analysis/) 同步更新。

## 依赖

```js
import freeGlobal from './freeGlobal.js'; // 全局 global 变量
```

## 源码

```js
/** 检测自由变量 `exports` */
const freeExports = typeof exports == 'object' && exports !== null && !exports.nodeType && exports;

/** 检测自由变量 `module` */
const freeModule = freeExports && typeof module == 'object' && module !== null && !module.nodeType && module;

/** 检测 CommonJS 扩展 `module.exports` */
const moduleExports = freeModule && freeModule.exports === freeExports;

/** 检测 Node.js 中自由变量  `process`*/
const freeProcess = moduleExports && freeGlobal.process;

/** 用于更快访问的 Node.js helper */
const nodeTypes = (() => {
  try {
    /* 检测 Node.js v10+ 中的 `util.types` helpers */
    const typesHelper = freeModule && freeModule.require && freeModule.require('util').types;
    return typesHelper
      ? typesHelper
      : /* 早于 v10 的 Node.js 的旧版 process.binding（'util'）. */
        /* Node.js弃用代码：DEP0103。 */
        freeProcess && freeProcess.binding && freeProcess.binding('util');
  } catch (e) {}
})();
```

## 相关链接

- [How to check whether a script is running under node.js](https://stackoverflow.com/questions/4224606/how-to-check-whether-a-script-is-running-under-node-js)
