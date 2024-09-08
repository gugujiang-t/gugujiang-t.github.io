---
sidebar_position: 3
---

# 模块

## JavaScript 模块发展简史

起初（1995 年），js 不支持任何模块系统。这时问题出现了：

1. 一切都在全局命名空间中声明，导致应用非常难以构建和弹性收缩。
2. 变量名很容易发生冲突。
3. 而且，无法显示指明各个模块对外开放哪些 API，导致难以分清模块的哪些部分是可供外部使用的，哪些部分是不想开放细节的。

当时的解决方案，是使用对象或立即调用的函数表达式模拟模块，把对象或 IIFE 附加到全局对象 window 上。这个方案的问题是：我们知道加载和运行 js 会阻塞浏览器 UI，随着代码行的增多，这样的方式会导致浏览器变得越来越慢。

然后，提出了动态加载 js，也就是异步加载 js 的方案。异步加载要就解决 3 个问题：

1. 模块适当封装
2. 模块之间的依赖要显示声明，否则不知道需要加载什么模块，以及什么顺序
3. 每个模块在应用中都要有一个唯一标识符。

当时，NodeJS 的开发人员实现的模块系统，采用的是**CommonJS 模块标准**，就是我们熟知的这样

```javascript title="emailBaseModule.js"
var emailList = require("emailListModule");
var emailComposer = require("emailComposerModule");

module.exports.renderBase = function () {
  // ...
};
```

同时，有其他的库主推**AMD 模块标准**，这个标准支持的功能不变，还内置了一个构建系统，用于打包 js 代码，

像这样

```javascript
define("emailBaseModule", [
  "require",
  "exports",
  "emailListModule",
  "emailCopmposerModule",
], function (require, exports, emailListModule, emailCopmposerModule) {
  exports.renderBase = function () {
    // ...
  };
});
```

后来，Browserify 这个库让浏览器中也可以使用 CommonJS 了，因此 CommonJS 更加普及了起来。 但是 CommonJS 有一些问题：

1. require 必须同步调用
2. 使用 CommonJS 的代码在某些情况下不会做静态分析，因为 module.exports 可能出现在任何位置，require 的调用也可以在任何位置（比如永远不会执行到的 if 分支里），而且可能包含任意字符串和表达式。这就导致无法静态链接 js 程序，不能确认被引用的文件真实存在，也无法确保被引用的文件真的导出了它声称导出的代码。

因此，ECMAScript 语言第 6 版（即 ES2015）中创建了一个导入和导出引入的新标准，而且支持静态分析。这就是我们熟知的 **ES modules** 了，就像这样

```javascript title="emailBaseModule.js"
import emailList from "emailListModule";
import emailComposer from "emailComposerModule";

export function renderBase() {
  // ...
}
```

不过，不是每个 js 引擎都原生支持这个标准，所以需要把代码编译成支持的格式：

- NodeJS —— 编译成 CommonJS 格式
- 浏览器环境 —— 编译成全局或单个模块可加载的模式

## TS 中使用模块

### import、export

一般推荐在 ts 代码中使用 ES2015 的 import 和 export 句法，不要使用 CommonJS、全局或命名空间下的模块

```typescript
/// 具名导出
// a.ts
export function foo() {}
export function bar() {}
// b.ts
import { foo, bar } from "./a";
foo();
export let result = bar();

/// 默认导出
// c.ts
export default function meow(loudness: number) {}
// d.ts
import meow from "./c";
meow(11);

/// 通配符导入导出一切
// e.ts
import * as a from './a';
a.foo();
a.bar();
// f.ts
export * from './a'
export {result} from './b'
export meow from './c'

/// 导出类型和接口，且能自动推导
// g.ts
export let X = 3
export type X = {x: string}
// h.ts
import {X} from './g';
let a = X + 1  // X 代指值X
let b: X = {x: 'hello'}  // X代指类型X
```

### 动态导入

如果顶层要导入很多代码，从文件系统重导入解析编译求职，会发现很多时间，且会阻塞其他代码。所以在前端，可以把代码拆分（code splitting），把代码拆成一系列小一些的 javascript 文件。这样我们可以并行加载多块代码，降低网络请求的负担。

进一步地，可以做惰性加载分块（即在真正用到时才加载），进一步优化加载速度。这就是 **“动态导入”**， 用法如下

先在*tsconfig.json*中设置一下，因为 ts 只在 esnext 模块模式下支持动态导入：在`compilerOptions` 选项里设置`{"module": "esnext"}`

```typescript
// 将import作为一个函数调用，此时的返回结果是一个Promise
let locale = await import("locale_us-en");
```

### 使用 CommonJS 和 AMD 模块

写法上很类似 ES2015，

```typescript
import { something } from "./a/commonjs/module";
```

如果想使用默认导出，要借助通配符导入：

```typescript
import * as fs from "fs";

fs.readFile("some/file.txt");
```
