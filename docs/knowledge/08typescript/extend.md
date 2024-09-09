# 如何动态扩展对象

在 TypeScript 中，我们经常需要在运行时动态添加属性到对象上。这是因为 TypeScript 是一种静态类型语言，类型系统在编译时会检查代码的类型安全性，所以在编译时我们无法确定对象上将要添加哪些属性。在本文中，我们将讨论如何在 TypeScript 中为对象动态添加属性，以及这样做的一些注意事项。

1. 使用索引签名

```typescript
interface MyObj {
  [key: string]: any;
}
```

2. 使用 Object.assign

```typescript
const myObject = {};
const newObject = { b: "hello" };
Ojbect.assign(myObject, newObject);
```

注意：由于 Object.assign 是一种浅拷贝方法，它只会复制对象的属性，而不会复制属性值所属的对象

3. 使用 ES6 扩展运算符

4. 使用类型断言 `as`
