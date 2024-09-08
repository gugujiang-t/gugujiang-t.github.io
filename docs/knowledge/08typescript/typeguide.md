---
sidebar_position: 2
---

# 类型守卫

类型守卫是一种运行时检查变量类型的方法，确保变量是特定类型，细化类型，起到一个保护作用。

实现方法：`is`运算符

```typescript title='类型断言'
function isNumber(x: any): x is number {
  return typeof x === "number";
}
```

## 类型谓词 is 和 boolean 的区别在哪里呢？

ts 内置的类型细化的能力有限，只能细化当前作用域中变量的类型，一旦离开这个作用域，类型细化能力不会随之转移到新作用域中。所以在下面的`isString`函数中，输入参数在函数里的类型是 string，但是在新的作用域（`parseInt`函数里面），细化能力会消失，也就是说 ts 只知道`isString`函数返回一个布尔值，所以会报错。

### 直接使用 boolean 作为返回值类型

```typescript
function isString(a: unknown): boolean {
  return typeof a === "string";
}

function parseInt(input: string | number) {
  let formattedInput: string;
  if (isString(input)) {
    formattedInput = input.toUpperCase(); // Error: TS2339: Property 'toUpperCase' does not exist on type 'number'
  }
}
```

那么，我们就需要让类型检查器知道，当返回的布尔值是 true 的时候，标明我们传给 isString 函数的参数是一个字符串。那么就可以使用`is`运算符来定义类型

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

## 注意

1. 使用类型保护时，ts 会进一步缩小变量的类型
2. `is`不仅能定义基础数据类型，自定义的 type 也是可以定义的哦。
3. 类型守卫的好处是不用每一次在行内使用 typeof 和 instanceof 类型防护措施，简化代码，提升代码的重用性。
