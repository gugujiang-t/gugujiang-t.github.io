---
sidebar_position: 1
---

# 创建/初始化篇

## 初始化二维数组

初始化 `m*n` 且元素都为空的二维数组

```javascript
// 方法1： 使用Array.from、fill、map
const arr = Array.from({ length: m })
  .fill(null)
  .map(() => new Array(n));

// 方法2：使用new、fill、map
const arr2 = new Array(m).fill().map(() => new Array(n));

// 方法3：使用循环
let m = 3;
let n = 3;
let arr3 = [];
for (let i = 0; i < m; i++) {
  arr3[i] = new Array(n);
}
```

初始化 `m*n` 且元素都为 `0` 的二维数组

```javascript
// 在上面方法的基础上加上fill即可
const arr = Array.from({ length: m })
  .fill(null)
  .map(() => new Array(n).fill(0));

const arr2 = new Array(m)
  .fill(null)
  .map(() => new Array(n))
  .fill(0);

let m = 3;
let n = 3;
let arr3 = [];
for (let i = 0; i < m; i++) {
  arr3[i] = new Array(n).fill(0);
}
```

## 初始化集合 `set`

```js
let s = new Set();

// 具有初始值的Set集合
let initialValueSet = new Set(["value1", "value2", "value3"]);
// Set(3) {'value1', 'value2', 'value3'}
```
