---
sidebar_position: 0
---

# node 输入输出

本地刷题第一步，使用控制台输入输出，这个对于 JavaScript 有一点点点点麻烦，需要库`readline`。

```ts
import readline from "readline";
// 初始化输入输出流
const r = readline.createInterface({
  input: process.stdin, // 控制台输入
  output: process.stdout, // 控制台输出
});

/**
 * 全局变量区，控制输入
 */
let n;
let arr;
let first = 1;
//...

/*
 * 使用监听事件的方式处理输入
 */
// line事件是监听每一行的输入
r.on("line", (line) => {
  console.log(line);
  // 各种操作...... 注意line是字符串
  // 比如分割并转化成数字
  // const inputs = line.split(' ').map(Number)

  // 最后使用close方法关闭输入输出流
  r.close();
});

// 监听close事件，退出node程序
r.on("close", () => {
  process.exit(0);
});
```

然后在控制台运行：

```bash
% node 文件名
```

跑起来后，就可以在控制台输入 input 了

如果需要从文件输入，那么可以配合 `fs` 库，并且更改 `input参数` 的值为文件输入流

```ts
import readline from "readline";
import fs from "fs";

const fileStream = fs.createReadStream("./博弈论图论-米哈游秋招-input.txt");

const r = readline.createInterface({
  input: fileStream, // 改为fs文件流
  output: process.stdout,
});
// 下面就相同了
// ...
```
