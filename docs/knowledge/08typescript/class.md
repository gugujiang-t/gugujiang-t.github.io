---
sidebar_position: 1
---

# 类和接口

两者区别：

1. type 用`&`继承；interface 用 `extends` 继承
2. type 更加通用，右边可以是任何类型，包括类型表达式；接口声明中，右侧必须为结构
3. 扩展接口时，typescript 将检查扩展的接口是否可赋值给被扩展的接口。例如：

```typescript
interface A {
  bad(x: string): string;
}
interface B extends A {
  bad(x: string): string; // 报错：
}
```

但是使用交集类型时不会出现这个问题，最终的结果是重载 bad 的签名。所以在建模对象类型的继承时，往往 用接口，可以检查可赋值性

4. 声明合并特性：同一个作用域中的同名接口将自动合并；而同一个作用域中的多个同名类型别名会导致编译时错误
