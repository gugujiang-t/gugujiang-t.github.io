---
sidebar_position: 5
---

# Map 和 Set

## Map

### 特点

1. 键可以是任意的类型，包括对象、函数，值也可以是任意的数据类型
2. 键值对可以被枚举：可以通过`entries()` `keys()` `values()` `forEach()` 来枚举，但不支持`for...in`
3. 键值对可以迭代：可以通过`for...of`循环来迭代遍历
4. 持久引用：存储在 Map 中的对象作为键，会保持对其的引用，即使没有其他引用指向这些对象，也不会被垃圾回收机制回收
5. 内存：因为强引用，所以可能导致内存泄露

底层：

### 常用方法

```javascript
const map = new Map(); // 创建
map.set("key1", "value1"); // 添加key
map.set({ id: 1 }, "object value");

console.log(map.get("key1")); // 输出 'value1'  通过key读取value
console.log(map.get({ id: 1 })); // 输出 'object value'

for (const [key, value] of map) {
  // for...of 遍历
  console.log(`${key}: ${value}`);
}

/*  迭代方法 */
const myMap = new Map([
  // 二维数组创建
  ["key1", "value1"],
  ["key2", "value2"],
  ["key3", "value3"],
]);

// 使用 for...of 循环  遍历key、value
console.log("Using for...of:");
for (const [key, value] of myMap) {
  console.log(`Key: ${key}, Value: ${value}`);
}

// 使用 .keys() 方法  遍历key
console.log("Using keys:");
for (const key of myMap.keys()) {
  console.log(`Key: ${key}`);
}

// 使用 .values() 方法  遍历value
console.log("Using values:");
for (const value of myMap.values()) {
  console.log(`Value: ${value}`);
}

// 使用 .entries() 方法 遍历key、value
console.log("Using entries:");
for (const [key, value] of myMap.entries()) {
  console.log(`Key: ${key}, Value: ${value}`);
}

// 使用 .forEach() 方法 遍历value
console.log("Using forEach:");
myMap.forEach((value, key) => {
  console.log(`Key: ${key}, Value: ${value}`);
});
```

### 🔗 面试题：Map VS WeakMap

Map 的特点在上面

WeakMap 的特点：

1. 键必须是对象（包括函数），不允许使用基本类型作为键，否则会报错 TypeError
2. 弱引用：键是弱引用的，意味着如果一个对象作为 key 存储在 WeakMap 中，如果该对象没有任何强引用指向它，那么这个 key 会被垃圾回收掉
3. 不可枚举：不能用`entries()` `keys()` `values()`
4. 不可迭代 不能用 `for...of` 循环
5. 内存：因为弱引用，所以不会导致内存泄露

**所以`Map` 和 `WeakMap` 的区别主要在三个方面：**

1. 键的类型：Map 支持任意类型的键，WeakMap 只支持对象类型的键
2. 引用类型：Map 中的键是强引用，WeakMap 中的键是弱引用
3. 可枚举性：Map 支持枚举，WeakMap 不支持枚举

代码说明强引用和弱引用的区别：

```javascript
/* weakmap 弱引用 */
// 创建一个 WeakMap 实例
const stateMap = new WeakMap();

// 创建几个对象
const obj1 = { id: 1, name: "Object1" };
var obj2 = { id: 2, name: "Object2" }; // 注意别用const ，const不能重新赋值的

// 向 WeakMap 中添加键值对
stateMap.set(obj1, { status: "active" });
stateMap.set(obj2, { status: "inactive" });

// 检查状态
console.log(stateMap.get(obj1).status); // 输出 'active'
console.log(stateMap.get(obj2).status); // 输出 'inactive'

// 释放 obj2 的引用，此时 obj2 可能会被垃圾回收
obj2 = null;

console.log(stateMap.get(obj2)); // 输出 undefined，因为 obj2 已经被垃圾回收
stateMap.has(obj2); // false
console.log(stateMap); // 打印的WeakMap不会有obj2这个属性了
//

/* map 强引用 */
const stateMap2 = new Map();

// 创建几个对象
const obj12 = { id: 1, name: "Object1" };
var obj22 = { id: 2, name: "Object2" }; // 注意别用const ，const不能重新赋值的

// 向 WeakMap 中添加键值对
stateMap.set(obj12, { status: "active" });
stateMap.set(obj22, { status: "inactive" });

// 检查状态
console.log(stateMap2.get(obj12).status); // 输出 'active'
console.log(stateMap2.get(obj22).status); // 输出 'inactive'

obj22 = null;

console.log(stateMap.get(obj22)); // undefined

stateMap.delete(obj22); // false 删不掉的，map中会一直有这个key
console.log(stateMap2); // 打印的Map中一直会有obj2这个属性了
```
