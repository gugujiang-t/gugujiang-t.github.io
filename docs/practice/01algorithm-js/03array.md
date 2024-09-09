---
sider-position: 3
---

# Array 技巧篇

## 排序

如果元素类型不为 Number，记得先 map 为 Number 哦~

```js
const arr = [6, 34, 2, 7, 10];
// 升序排序
const sortedArr = arr.sort((a, b) => a - b); // [2, 6, 7, 10, 34]
```

## 数组转成其他类型

### 数组转成 Set

```js
// 能够实现去重
let myArray = [1, 2, 2, 3];
let mySetFromArray = new Set(myArray); // Set(3) {1, 2, 3}
```

### 数组转成 Map

```js
// 需要是二维数组哦
let arrayOfEntries = [
  ["key1", "value1"],
  ["key2", "value2"],
  ["key3", "value3"],
];
let myMapFromArray = new Map(arrayOfEntries);
// Map(3) {'key1' => 'value1', 'key2' => 'value2', 'key3' => 'value3'}
```
