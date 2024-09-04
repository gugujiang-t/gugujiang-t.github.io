---
sidebar_position: 2
---

# 类型守卫

类型守卫是一种运行时检查变量类型的方法，确保变量是特定类型。

实现方法：`is`

```typescript title='类型断言'
function isNumber(x: any): x is number {
  return typeof x === "number";
}
```

## 类型谓词 is 和 boolean 的区别在哪里呢？

### 使用 `is` 类型保护

```typescript
function isString(test: any): test is string {
  return typeof test === "string";
}

function example(foo: any) {
  if (isString(foo)) {
    // 如果进入了这里，typescript编译器会认为foo已经是一个string类型的变量了
    console.log("it is a string" + foo);
    console.log(foo.length); // string function
    // 如下代码编译时会出错，运行时也会出错，因为 foo 是 string 不存在toExponential方法
    console.log(foo.toExponential(2));
  }
  // 编译不会出错，但是运行时出错
  console.log(foo.toExponential(2));
}
example("hello world");
```

### 使用 boolean 作为返回值类型

```typescript
function isString(test: any): test is string {
  return typeof test === "string";
}

// 编译不会出错，因为类型是any 类型检查会通过
function example(foo: any) {
  if (isString(foo)) {
    console.log("it is a string" + foo);
    console.log(foo.length); // string function
    console.log(foo.toExponential(2));
  }

  console.log(foo.toExponential(2));
}
example("hello world");
```

## 注意

1. 使用类型保护时，ts 会进一步缩小变量的类型
2. 类型保护的作用域仅仅在 if 后的块级作用域中生效
