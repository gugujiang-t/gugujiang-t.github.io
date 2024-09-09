---
sidebar_position: 2
---

# 运算篇

## 整数除法

### 使用`Math.floor()` 和 `Math.ceil()`

**🔆 注意**：如果结果为正数，使用`Math.floor()` ；如果结果为负数，则需要使用`Math.ceil()`

```javascript
let c = a / b;
c = c > 0 ? Math.floor(c) : Math.ceil(c);
```

### 使用 bigint 特性

bigint 在除法运算的时候，结果会向零取整，也就是说不会返回小数部分，行为类似 C 的整数除法哦，这个倒是挺好用的

```javascript
// 计算各个位上的数字之和
var num = 123456789;
num = BigInt(num);
var sum = 0n;
while (num != 0n) {
  sum += num % 10n;
  num = num / 10n;
}
console.log(sum); // 45

console.log(-33n / 10m); // -3n
```

## 浮点精度问题

```javascript
const float1 = 7.9;
const float2 = 0.8;
console.log(float1 - float2); //7.1000000000000005
```

解决方法：

### 使用 toFixed

parseFloat() 方法 将字符串或浮点数转换为浮点数，开头和结尾的空格是允许的，但如果用空格分割的字符串，只返回第一个数字。

toFixed() 方法将数字转换为字符串。
toFixed() 方法将字符串**四舍五入**为指定的小数位数。

注释：如果小数位数高于数字，则添加零。

```javascript
const float1 = 7.9;
const float2 = 0.8;
parseFloat(float1 - float2).toFixed(10); // 7.1000000000
parseFloat(float1 - float2).toFixed(2); // 7.10
parseFloat(float1 - float2).toFixed(); // 7
```

## 大数运算

可以用 bigint

```javascript
var x = 123456789n;
var y = 987654321n;
console.log(x * y); //121932631112635269n

// 使用toString转为字符串，这样结尾的n就去掉了
console.log((x * y).toString()); //121932631112635269
```
